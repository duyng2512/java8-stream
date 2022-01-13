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