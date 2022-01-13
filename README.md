# Java Stream

## What are streams?

> Streams are an update to the Java API that lets you manipulate collections of data in a declarative way (you express a query rather than code an ad hoc implementation for it). For now you can think of them as fancy iterators over a collection of data. In addition, streams can be processed in parallel transparently, without you having to write any multi threaded code

```java
/* For example before java 8, we want to filter by weight and sort acs and get name*/
static void filterBefore8(List<Animal> list){
        List<Animal> result = new ArrayList<>();
        for (Animal animal: list){
            if (animal.getWeight() > 100) {
                result.add(animal);
            }
        }

        result.sort(new Comparator<Animal>() {
            @Override
            public int compare(Animal o1, Animal o2) {
                return Integer.compare(o1.getWeight(), o2.getWeight());
            }
        });

        List<String> resultName = new ArrayList<>();
        for(Animal animal : result){
            resultName.add(animal.getName());
        }
        System.out.println(resultName.toString());
    }

/* Such code in java 8 stream can be conside as */
    static void filterAfter8(List<Animal> animals){
        List<String> result = animals.stream()
                                    .filter(a -> a.getWeight() > 100)
                                    .sorted(Comparator.comparing(Animal::getWeight))
                                    .map(Animal::getName)
                                    .collect(Collectors.toList());
        System.out.println(result.toString());
    }
    
    
/* To exploit a multicore architecture and execute this code in parallel, you need only change
stream() to parallelStream() */
    static void filterAfter8(List<Animal> animals){
        List<String> result = animals.parallelStream()
                                    .filter(a -> a.getWeight() > 100)
                                    .sorted(Comparator.comparing(Animal::getWeight))
                                    .map(Animal::getName)
                                    .collect(Collectors.toList());
        System.out.println(result.toString());
    }

```

![](img/1_.png)

## Getting started with streams

> **Stream** is a sequence of element from a source (File, IO, network, or collection, ..) that support data processing operation.

- **Characteristic** of a stream:
    1. Sequence of data
    2. Source
    3. Data processing operation
    4. Pipe line
    5. Internal iterator.


## Stream vs collections:
- One big different between stream and collections is when they are computed, collection is an in-memory data structure that hold values meanwhile a stream is an sequence of data coming from a source. Collection is eager computed while stream is computed on demand. 

- As an analogy, we can think of collection is a list of movie frame store in DVD, any frame is store upfront and user can select any frame, meanwhile a stream is a sort of video loading from internet, only a few frame is load (and a few more is buffer) at particular time.

![](img/2_.png)

- Stream also have some characteristic such as:
    1. transversal only one.
    ```java
    Stream<Animal> stream = animals.stream();
        stream.forEach(System.out::println);
        stream.forEach(System.out::println);
        
        
    Exception in thread "main" java.lang.IllegalStateException: stream has already been operated upon or closed
	at java.base/java.util.stream.AbstractPipeline.sourceStageSpliterator(AbstractPipeline.java:279)
	at java.base/java.util.stream.ReferencePipeline$Head.forEach(ReferencePipeline.java:658)
	at stream.Intro.main(Intro.java:60)
    ```
    
    2. External vs internal iteration
    
![](img/3_.png)
    
## Stream operation: