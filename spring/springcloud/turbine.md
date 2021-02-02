## Turbine

### 1.turbine-core-1.0.0.jar

#### 1.1 InstanceObservable

```java
//默认定时器间隔时间
private final DynamicIntProperty pollDelayMillis = DynamicPropertyFactory.getInstance().getIntProperty("turbine.discovery.pollDelayMillis", 60000);

 public void start(InstanceDiscovery iDiscovery) {
        if (started.get()) {
            throw new RuntimeException("InstanceDiscovery already started");
        }
        if (iDiscovery == null) {
            throw new RuntimeException("InstanceDiscovery is null");
        }
        instanceDiscovery = iDiscovery;
        logger.info("Starting InstanceObservable at frequency: " + pollDelayMillis.get() + " millis");
        timer.schedule(producer, 0, pollDelayMillis.get());
        started.set(true);
    }


 /**
     * TimerTask that runs at a periodic rate.
     */
    private final TimerTask producer = new TimerTask() {

        @Override
        public void run() {
            if(observers == null || observers.size() == 0) {
                logger.info("No observers for InstanceObservable, will try again later");
                return; 
            }

            // get the refreshed cluster
            List<Instance> newList = null;
            try {
                newList = getInstanceList();

                logger.info("Retrieved hosts from InstanceDiscovery: " + newList.size());
                if(newList.size() < 10) {
                    logger.debug("Retrieved hosts from InstanceDiscovery: " + newList);
                }

                // The total set of hosts in the previous round (both sets up and down combined)
                List<Instance> previousList = new ArrayList<Instance>(currentState.get().hostsUp);

                // Now subtract out the total set of new hosts found from the previous list. 
                // This should give us any hosts that disappeared quickly between this round and the previous      round     
                previousList.removeAll(newList);

                logger.info("Found hosts that have been previously terminated: " + previousList.size());

                // clear the previous state
                CurrentState newState = new CurrentState();

                // set the current state
                for(Instance host: newList) {
                    if(host.isUp()) {
                        newState.hostsUp.add(host);
                    } else {
                        newState.hostsDown.add(host);
                    }
                }

                // add hosts that disappeared between this round and the previous round only. 
                newState.hostsDown.addAll(previousList);

                logger.info("Hosts up:" + newState.hostsUp.size() + ", hosts down: " + newState.hostsDown.size());

                // setting the atomic reference
                currentState.set(newState);

                // Notify watchers of fleet changes
                for(InstanceObserver watcher: observers.values()) {
                    if(currentState.get().hostsUp.size() > 0) {
                        try {
                            watcher.hostsUp(currentState.get().hostsUp);
                        } catch(Throwable t) {
                            logger.error("Could not call hostUp on watcher: " + watcher.getName(), t);
                        }
                    }
                    if (currentState.get().hostsDown.size() > 0) {
                        try {
                            watcher.hostsDown(currentState.get().hostsDown);
                        } catch(Throwable t) {
                            logger.error("Could not call hostDown on watcher: " + watcher.getName(), t);
                        }
                    }
                }

                updateHostsCountsPerCluster(currentState.get().hostsUp);

                heartbeat.incrementAndGet();

            } catch (Throwable t) {
                logger.info("Failed to fetch instance info, will continue to run and will try again later", t);
                return;
            }
            return;
        }
    };
```

### 2.配置

```
turbine:
  app-config: winning-bmts-mdm-auth
#  cluster-name-expression: "default"
  combine-host-port: true
  discovery:
      pollDelayMillis: 600000
```

```
/Library/Java/JavaVirtualMachines/jdk1.8.0_211.jdk/Contents/Home/bin/java -agentlib:jdwp=transport=dt_socket,address=127.0.0.1:58818,suspend=y,server=n -Dwinning.devops.debug=true -Dwinning.devops.url=http://172.16.6.210:3001/api/apis/config/out_dev/1010 -Dwinning.devops.id=0 -Dapollo.bootstrap.enabled=false -Dspring.cloud.consul.discovery.register=false -Dfeign.hystrix.enabled=false -Dspring.cloud.consul.discovery.tags=W-FLOW=canary,W-SEQ=7205 -Dwinning.devops.id=275 -Dapollo.bootstrap.enabled=true -Dspring.cloud.consul.discovery.register=true -Dspring.cloud.consul.discovery.tags=W-FLOW=canary,W-SEQ=7205 -Turbine.discovery.pollDelayMillis=5000 -javaagent:/Users/qinkai/Library/Caches/IntelliJIdea2019.1/captureAgent/debugger-agent.jar -Dfile.encoding=UTF-8 -classpath /private/var/folders/vp/01cgn2893pgbynxkx5h_yrm40000gn/T/classpath856640148.jar com.winning.base.akso.WinningSpringBootApplication
```

