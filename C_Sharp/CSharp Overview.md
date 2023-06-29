
## Virtual Properties
[Dotnet Docs - Virtual Properties](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/virtual)
The `virtual` keyword is used to modify a method, property, indexer, or event declaration and allow for it to be overridden in a derived class. For example, this method can be overridden by any class that inherits it:

```
public virtual double Area()
{
    return x * y;
}
```

You cannot use the `virtual` modifier with the `static`, `abstract`, `private`, or `override` modifiers.

## Delegates

A type safe function pointer.
It is a reference type.

#todo
### Variance in Delegates

#todo
### Covariance in Delegates

#todo
### Contravariance in Delegates


# Examples of Delegate

```C#
class Program{
	delegate void LogDel(string text);

	static void Main(string[] args){
		Log log = new Log();

		LogDel logDel = new LogDel(log.LogTextToScreen);

		Console.WriteLine("Please enter name:")

		var name = Console.ReadLine();

		logDel(name);

		Console.ReadKey();
	}	
}
	public class Log
	{
		public void LogTextToScreen(string text)
		{
			Console.WriteLine($"{DateTime.Now}: {text}");
		}

	public void LogTextToFile(string text) 
	{
		using(StreamWriter sr = new(Path.Combine(AppDomain.CurrentDomain.BaseDirectory, "Log.txt", true))){
			sw.Write(text);
		}
	}
}
```
