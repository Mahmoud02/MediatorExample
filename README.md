# MediatorExample

## before explaining Mediator, let's see these cases 
### Case1: if object A wants to send a message to object B!!
maybe the first solution you think about it is to CREATE  FUNCTION in the "A" class and name it "Send" and this function will receive an object of type B. then the "B" object will handle the msg.
```c#
    class A {
      ---
      ---
      send (B obj ){
      }
    };
    Class B{
    };
```    
ok, that works and maybe good in small solutions.

### Case2 :
but what happens if we want to add more Classe like C, D, Z an and so on and we want A to send messages to all of them?
maybe we create an interface "IMessage" and make all Classes implement it. then the "Send" function will receive an object of type "Imessage".
```c#
  interface IMessage{
    void handleIncomingMsg(string msg);
  }
  class A{
    send (Imessage obj )
  }
 ```  
### Case3:
in the previous examples: only Class A sends msg to all other classes.
but what we should do if we want all classes to communicate with each other vice versa??
we can create a list of type IMessage in each class and we will register all other objects in it.
```c#
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
  ```
and so on for other classes.\
That is a lot of code and we add the same code to  other classes.\
The classes do a communication task and it should only be responsible for one thing.\
**In case we have many objects that interact with each other, we end up with very complex and nonmaintainable code. That’s why it is a good case to use the Mediator design pattern**.\
<img src="https://user-images.githubusercontent.com/18700494/109394273-dc839700-792e-11eb-97fd-ced9467b3e66.png" width="600" height="300"/>


# let's introduce the Mediator Pattern 
Mediator is a behavioral design pattern that lets you reduce chaotic dependencies between objects. The pattern restricts direct communications between the objects and forces them to collaborate only via a mediator object.
## let's define each class and its role
<img src="https://user-images.githubusercontent.com/18700494/109396159-eca07400-7938-11eb-948d-72102a2f4f1b.png" width="60%" height="400"/>

### Mediator [interface or abstarct class]
```c#
 public abstract class Mediator
    {
        //here we define the methods that will be used by Colleague objects to communicate.
        public abstract void Send(string message, Colleague colleague);
    }
```
### Mediator [concrete class]
```c#
public class ConcreteMediator : Mediator
        //here we manage every thing.
        //knows and maintains its colleagues.
        //encapsulates the interaction logic between Colleague objects.
        //the mediator responsible for the creation and destruction of Colleague objects.
        
        private List<Colleague> colleagues = new List<Colleague>();

        public void Register(Colleague colleague)
        {
            colleague.SetMediator(this);
            this.colleagues.Add(colleague);
        }

        public T CreateColleague<T>() where T : Colleague, new()
        {
            var c = new T();
            c.SetMediator(this);
            this.colleagues.Add(c);
            return c;
        }

        public override void Send(string message, Colleague colleague)
        {
           
            this.colleagues.Where(c => c != colleague).ToList().ForEach(c => c.HandleNotification(message));

        }
    }
```
### Colleague [abstract class]
```c#
public abstract class Colleague
    {
        //defines the abstract class holding a single reference to the Mediator.
        //should store a reference to the mediator object
        //each Colleague class knows its Mediator object.
        //each colleague communicates with its mediator whenever it would have otherwise communicated with another colleague.
        
        protected Mediator mediator;

        internal void SetMediator(Mediator mediator)
        {
            this.mediator = mediator;
        }

        public virtual void Send(string message)
        {
            this.mediator.Send(message, this);
        }

        public abstract void HandleNotification(string message);
    }
```
### Colleague [concrete Colleague class]
```c#
    public class Colleague1 : Colleague
        {
            public override void HandleNotification(string message)
            {
                Console.WriteLine($"Colleague1 receives notification message: {message}");
            }
        }
```
### Colleague [concrete Colleague class]
```c#
    public class Colleague2 : Colleague
        {
            public override void HandleNotification(string message)
            {
                Console.WriteLine($"Colleague1 receives notification message: {message}");
            }
        }
```
### Main
```c#
    class Program
    {
        static void Main(string[] args)
        {
            var mediator = new ConcreteMediator();
            var c1 = mediator.CreateColleague<Colleague1>();
            var c2 = mediator.CreateColleague<Colleague2>();
            c1.Send("Hello, World! (from c1)");
            c2.Send("hi, there! (from c2)");
        }
    }
```
### result
<img src="https://user-images.githubusercontent.com/18700494/109396907-8fa6bd00-793c-11eb-94cf-f191d4880882.png" height="700"/>

