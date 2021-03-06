# publishqueue-library-spark
Delays (puts in queues, thus the name) the calling of cloud events (via Particle.publish) so it does not pass the limits defined by Particle
  As defined by Particle in the [Particle.publish()](https://docs.particle.io/reference/firmware/photon/#particle-publish-) documentation: 

>"NOTE: Currently, a device can publish at rate of about 1 event/sec, with bursts of up to 4 allowed in 1 second. Back to back burst of 4 messages will take 4 seconds to recover."

it will record the time of last publish event and delay the next publish event until enough time as passed to guarantee the limits above are not hit.

This is a firware library to work with https://www.particle.io/. Was tested with Photon devices.
#Usage
You'll start by declarint the object with a parameter of the minimum elapsed time between publish events
```c++
PublishQueue pubQueue(1000);
```
Then you need to publish events to this queue, instead of submitting them directly to Particle. 
```c++
pubQueue.Publish("EventName1","11111");
```
Finally, you will need to call the Process() method inside your loop. The library will evolve to use Timers in the future, but for now you'll have to call this method to get things going.
```c++
pubQueue.Process();
```

#Example
Check [this code](https://github.com/tiagonmas/publishqueue-library-spark/tree/master/firmware/examples) for a working sample of using this library.

```c++

PublishQueue pubQueue(1000);

void setup()
{
    pubQueue.Publish("EventName1","11111");
    pubQueue.Publish("EventName2","22222");
    pubQueue.Publish("EventName3","33333");
    pubQueue.Publish("EventName4","44444");
    pubQueue.Publish("EventName5","55555");
    pubQueue.Publish("EventName6","66666");
    pubQueue.Publish("EventName7","77777");
    pubQueue.Publish("EventName8","88888");
    pubQueue.Publish("EventName9","99999");
    pubQueue.Publish("EventName10","99990");
    pubQueue.Publish("EventName11","99991");
    pubQueue.Publish("EventName12","99992");
    pubQueue.Publish("EventName13","99993");
    pubQueue.Publish("EventName14","99994");
    pubQueue.Publish("EventName15","99995");
    pubQueue.Publish("EventName16","99996");
}   
void loop()
{
    pubQueue.Process();
}
```


#Result of running this example
notice that the event dates are 1s appart.

'''
event: EventName1
data: {"data":"11111","ttl":"60","published_at":"2016-03-04T18:17:39.529Z","coreid":"1f0036000847343432313031"}

event: EventName2
data: {"data":"22222","ttl":"60","published_at":"2016-03-04T18:17:49.534Z","coreid":"1f0036000847343432313031"}


event: EventName3
data: {"data":"33333","ttl":"60","published_at":"2016-03-04T18:17:59.539Z","coreid":"1f0036000847343432313031"}

event: EventName4
data: {"data":"44444","ttl":"60","published_at":"2016-03-04T18:18:09.544Z","coreid":"1f0036000847343432313031"}

event: EventName5
data: {"data":"55555","ttl":"60","published_at":"2016-03-04T18:18:19.549Z","coreid":"1f0036000847343432313031"}


event: EventName6
data: {"data":"66666","ttl":"60","published_at":"2016-03-04T18:18:29.554Z","coreid":"1f0036000847343432313031"}

event: EventName7
data: {"data":"77777","ttl":"60","published_at":"2016-03-04T18:18:39.561Z","coreid":"1f0036000847343432313031"}

event: EventName8
data: {"data":"88888","ttl":"60","published_at":"2016-03-04T18:18:49.564Z","coreid":"1f0036000847343432313031"}


event: EventName9
data: {"data":"99991","ttl":"60","published_at":"2016-03-04T18:18:59.569Z","coreid":"1f0036000847343432313031"}

event: EventName10
data: {"data":"99992","ttl":"60","published_at":"2016-03-04T18:19:09.574Z","coreid":"1f0036000847343432313031"}

event: EventName11
data: {"data":"99993","ttl":"60","published_at":"2016-03-04T18:19:19.579Z","coreid":"1f0036000847343432313031"}
'''
#Future version
* Implement Timers so the user does not have to call Publish within the loop.
* Implement callbacks so user can get result of publish event (if it was succesfull or not)
* Implement priority queues so more important events can go to front of queue
