# Protocolize
A lightweight BungeeCord protocol framework supporting items.
## Purpose
Protocolize had been developed to raise the amount of game changing possibilities on BungeeCord platforms. With Protocolize it is possible to create custom inventories or interact with the player inventory itself. Protocolize is also ready for future upscaling! It is easy to self-implement custom packets using the *de.exceptionflug.protocolize.AbstractPacket* class.
## Compatibility
Protocolize has no currently known compatibility issues yet.
## How To Use
Please take a look at the examples which I made: https://github.com/Exceptionflug/protocolize/tree/master/protocolize-examples/src/main/java/de/exceptionflug/protocolize/example

### Listen for incoming and outgoing packets
At first you need to create a new PacketListener (eg. when you want to interact with chat messages)
```java
public class MyPacketListener extends PacketAdapter<Chat> {  
  public MyPacketListener() {  
    super(Stream.UPSTREAM, Chat.class);  
  }
}
```
and register it.
```java
ProtocolAPI.getEventManager().registerListener(new MyPacketAdapter());
```
Now you can override the following methods to access the packets.
```java
@Override  
public void receive(final PacketReceiveEvent<Chat> event) {  
  // Receive on UPSTREAM means receiving a packet from a player  
  // Receive on DOWNSTREAM means receiving a packet from a server a player is connected to
}  
  
@Override  
public void send(final PacketSendEvent<Chat> event) {  
  // Sending on UPSTREAM means sending a packet to a player  
  // Sending on DOWNSTREAM means sending a packet to a server a player is connected with
}
 ```
 For example if we want all actionbar messages printing in the chat instead we can use the following logic on `Stream.DOWNSTREAM`:
 ```java
public class MyPacketListener extends PacketAdapter<Chat> {  
  
   public MyPacketListener() {  
     super(Stream.DOWNSTREAM, Chat.class);  
   }  
  
   @Override  
   public void receive(final PacketReceiveEvent<Chat> event) {  
     final Chat packet = event.getPacket();  
     if(packet.getPosition() == 2) { // Position 2 means actionbar  
       packet.setPosition((byte) 0); // Set to normal chat  
       event.markForRewrite(); // We need to mark the packet for rewriting after we changed fields in the packet class. This is only needed when receiving packets.  
     }  
   }  
}
 ```