# Java #
## Migration from 8 ➞ 11 ##
![boar](giphy.gif)
---



## Java Platform Module System ##

* Reliable configuration
* Strong encapsulation
* Scalable Java platform
* Greater platform integrity
* Improved performance

---

## Module ##

a uniquely named, reusable group of related packages, as well as resources (such as images and XML files) and a *module descriptor*

---

## Module descriptor ##

* requires
* exports
* provides
* with
* uses

Note: In src directory

---

## JShell ##

The `jshell` tool provides an interactive 
command-line interface for evaluating declarations, statements, and 
expressions of the Java programming language.

---

## Factory Methods for Immutable Collections ##

```java
List<String> stringList = List.of("one", "two", "three");
Map.of(1, "one", 2, "two", 3, "three");
Map.ofEntries(Map.entry(1, "one"));
Set.of("one", "two", "three");

List<String> copyList = List.copyOf(stringList);
// unmodifiable copy of the given collection
```

---

## Private Methods in Interfaces ##

```java
public interface Logging {
	default void logInfo(String message) {
		log(message, "INFO");
	}
	default void logError(String message) {
		log(message, "ERROR");
	}
	private void log(String msg, String prefix) {
		var log = String.format("[%s] %s", prefix, msg);
        System.out.println(log);
	}
}
```

---

## Waited for it too long - `var` ##

```java
final var bestTeamEver = "DZIX";
final var repository = 
    DecoratorFilterInterpreterParameterTestsHttpManagerRepository();
```

---

## VAR is not a keyword ##

* It is a Reserved Type name.  
* Backwards compatibility - any "var" in existing code isn't affected

---

## Don't use var when ##

* you don't know the returned type

```java
final var result = process.run();
// I wanted `ProcessResult`
// But I have a `ProcessResultContainer`
```

* you want interface type

```java
final var array = new ArrayList<String>();
// array is type ArrayList<String>
```

* you need IntelliJ to help you

```java
final var result = new *Ctrl+Space*
// now what?...
```

---

## Some other examples ##

```java
var i, j = 0; // Invalid
var x = null; // Invalid

// Valid
Function<Group, GroupDto> toDto = 
    (@Valid final var g) -> new GroupDto(g.name); 

// Valid, ┐( ͡° ʖ̯ ͡°)┌
var list = new ArrayList<>()

// Invalid
var exceptionSupplier = () -> new IllegalArgumentException();
// Any lambda expression needs an explicit target-type
// No fancy functional programming... ( T ʖ̯ T)
```

---

## HttpClient ##
```java
HttpRequest request = HttpRequest.newBuilder()
	.uri(URI.create("http://dzix-rulez"))
	.GET()
	.build();

HttpResponse<String> response = 
	httpClient.send(request, BodyHandlers.ofString());
```

---

## Try-with-resources ##

```java
BufferedReader br = new BufferedReader("...");
try (br) {
   // Use br
} catch (IOException e) {
    e.printStackTrace();
}
```

---

## Optional ##

```java
final var opt = Optional.empty();
opt.isEmpty() // true

var value = opt.orElseThrow() 
// throws NoSuchElementException

opt.ifPresentOrElse(this::use, this::otherwise);
// Consumer<? super T> action, Runnable emptyAction

final var result = service.getLast()
.or(() -> Optional.of(current))  // or
.stream()                        // stream
.map(this::resultAsString)
.collect(Collectors.joining());
```

---

## Predicate ##

```java
List.of("one", "two", "three").stream()
    .filter(not(it -> it.startsWith("t")))
    .collect(Collectors.toUnmodifiableList());
```

---

## NullStreams ##

 Returns an InputStream that reads no bytes.

```java
InputStream nullInputStream()
OutputStream nullOutputStream()
Reader nullReader()
Writer nullWriter()
```

---

## Now with charset ##
```java
public FileReader(String fileName, Charset charset)
public FileReader(File file, Charset charset)

public FileWriter(String fileName, Charset charset)
public FileWriter(String fileName, Charset charset,
	boolean append)
public FileWriter(File file, Charset charset)
public FileWriter(File file, Charset charset,
	boolean append)
```

---

## Files ##
```java
String readString(Path) // UTF-8
String readString(Path, Charset)
Path writeString(Path, CharSequence, OpenOption[]) // UTF-8
Path writeString(Path, CharSequence, Charset, OpenOption[])
```

---

## Strings ##
```java
String isBlank() 
// String is empty or contains only white space codepoints

String lines() 
// String<Stream> of lines divided by "\n", "\r", or "\r\n".

String repeat()
"DZIX ".repeat(3).
// "DZIX DZIX DZIX "
```

---

## Collection into array ##
```java
// before Java 11
List<String> list = /*...*/;
Object[] objects = list.toArray();
String[] strings = list.toArray(new String[0]);

// now
String[] strings_fun = list.toArray(String[]::new);
```

---

![boar](boar.gif)

---

## Garbage Collectors `UseSerialGC` ##

* If the application has a small data set (up to approximately 100 MB), then select the serial collector with the option -XX:+UseSerialGC.
* If the application will be run on a single processor and there are no pause-time requirements, then select the serial collector with the option -XX:+UseSerialGC.

---

## Garbage Collectors ##

* If (a) peak application performance is the first priority and (b) there are no pause-time requirements or pauses of one second or longer are acceptable, then let the VM select the collector or select the parallel collector with -XX:+UseParallelGC.
* If response time is more important than overall throughput and garbage collection pauses must be kept shorter than approximately one second, then select a mostly concurrent collector with -XX:+UseG1GC or -XX:+UseConcMarkSweepGC.

---

## Garbage Collectors ZGC ##
* If response time is a high priority, and/or you are using a very large heap, then select a fully concurrent collector with -XX:UseZGC.

---

## Others ##
* JVM parameters - logging
* Predicate asMatchPredicate()
* PriorityBlockingQueue & PriorityQueue
    * void forEach(Consumer)
    * boolean removeAll(Collection)
    * boolean removeIf(Predicate)
    * boolean retainAll(Collection)