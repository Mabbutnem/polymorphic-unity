# Polymorphic Unity

Unity inspector lack on support polymorphism. This library allow you, to define all the children class of a base class. The inspector will serialize their fields dynamically depending on the current choosen child.

:white_check_mark: Supports :
* Concrete class, abstract class and interface parent.
* MonoBehavior and ScriptableObject inspectors.
* Nested polymorphic objects.
* List of polymorphic objects.
* Singletons

:x: Does not support :
* Array, Dictionnary, Queue or Set of polymorphic objects.

## Installation

Copy files from Assets/Editor/Plugins/Polymorph into your Assets/Editor/Plugins/Polymorph path of your project.

Then, copy files from Assets/Scripts/Plugins/Polymorph into your Assets/Scripts/Plugins/Polymorph path of your project.

## Usage

### Define childrens

You can define some children for your Foo class as follows :

```csharp
[Serializable]
[Polymorphic(
    typeof(Foo),
    typeof(Foo1),
    typeof(Foo2)
)]
public class Foo : IPolymorphic
{
    [SerializeField]
    private int field1;
    public int Field1 { get => field1; }

    [SerializeField]
    private string field2;
    public string Field2 { get => field2; }
}

public class Foo1 : Foo
{
    [SerializeField]
    private string field3;
    public string Field3 { get => field3; }
}

public class Foo2 : Foo
{
    [SerializeField]
    private string field4;
    public string Field4 { get => field4; }
}
```
The base class Foo must be ```[Serializable]``` and implement the ```IPolymorphic``` interface.

Then, to define available childrens for your Foo class, you have to add the ```Polymorphic``` attribute with the right types in it. In this example, types are Foo, Foo1 and Foo2.

### Use in inspector

To use the polymorphic class Foo in your inspector (MonoBehavior for instance), you have to declare it as follows :

```csharp
public class MonoBehaviorExample : MonoBehaviour
{
   [SerializeReference]
   public Foo singleFoo = new Foo();
}
```

You MUST add the ```[SerializeReference]``` attribute to your declaration to be able to use polymorphism.

It is also advised to initialize by default your field to prevent some bugs related to the ```[SerializeReference]``` attribute. In this example, singleFoo is initialized by default to ```new Foo()```.

You can see the following serialized object in your inspector :

![missing image](https://github.com/Mabbutnem/polymorphic-unity/blob/images/serialized.png?raw=true)

You can choose the type you want to serialize using the dropdown :

![missing image](https://github.com/Mabbutnem/polymorphic-unity/blob/images/dropdown.png?raw=true)

### List in inspector

You can also use a polymorphic list as follows :

```csharp
public class MonoBehaviorExample : MonoBehaviour
{
   [SerializeReference]
   public List<Foo> manyFoos = new List<Foo>();
}
```
:warning: Carreful ! The ```[SerializeReference]``` attribute in List brings an unwanted behavior. The reference of the nearest top object will be used when adding new element to a list.

Before playing with his fields, be sure to click on the dropdown child selector to create a new instance of this object.

## Advanced usage

### Data resilience

### Factories

## License
[MIT](https://choosealicense.com/licenses/mit/)
