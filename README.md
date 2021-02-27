# MediatorExample
Mediator is a behavioral design pattern that lets you reduce chaotic dependencies between objects. The pattern restricts direct communications between the objects and forces them to collaborate only via a mediator object.


## before explaining Mediator, let's see these cases 
### Case1: if object A wants to send a message to object B!!
the first solution you think about it is to CREATE  FUNCTION in the "A" class and name it "Send" and this function will receive an object of type B. then the "B" object will handle the msg.
  :class A {
    ---
    ---
    send (B obj ){
    }
  }
  Class B{
  }:
ok, that works and maybe good in small solutions.

### Case2 :
but what happens if we want to add more Classe like C, D, Z an and so on and we want A to send messages to all of them?
maybe we create an interface "IMessage" and make all Classes implement it. then the "Send" function will receive an object of type "Imessage".
  interface IMessage{
    void handleIncomingMsg(string msg);
  }
  class A{
    send (Imessage obj )
  }
### Case3:
in the previous examples: only Class A sends msg to all other classes.
but what we should do if we want all classes to communicate with each other vice versa??
so we can make a list of type IMessage in each class and we will register all other objects in it.
ex:
  class A{
    List<Imessage> otherobjectsList ;

    void regiesterObjectInTheList(Imessage obj){
      otherobjectsList.add(obj); 
    }
    send( ){
      foreach(obj in otherobjectsList  )
        obj.handleIncomingMsg(msg);
    }
  }
  class B{
    List<Imessage> otherobjectsList ;

    void regiesterObjectInTheList(Imessage obj){
      otherobjectsList.add(obj); 
    }
    send( ){
      foreach(obj in otherobjectsList  )
        obj.handleIncomingMsg(msg);
    }
  }
and so on for other classes.
that is a lot of code and we add the same code to each other classes.
The classes do a communication task and it should only be responsible for one thing.
and the communication will be like that.
<img src="https://user-images.githubusercontent.com/18700494/109394273-dc839700-792e-11eb-97fd-ced9467b3e66.png" width="600" height="300"/>


so here let's introduce the Mediator Pattern 
## let's define each class and its role
<img src="https://user-images.githubusercontent.com/18700494/109392932-b0184c80-7927-11eb-9605-337ca371bbf3.png" width="600" height="300"/>
