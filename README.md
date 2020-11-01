# Koala 

First of all, thank you very much for giving me this opportunity, and I  appreciate it. I have to say that I had lots of fun doing this test. Because I still need to do my regular job during the week, so I spent most of the nights to complete the test. 

The first part like a warm-up exercise and it didn't take too much time at all, and I only put my answers in here in case someone else may accidentally see the question.  

Like what I expected that most of the fun comes from doing the weather widget project, and I put a great effort into the project and try to design and complete it as a production-grade application. Like all the other projects, there is always something that could be improved, but I am happy to release it now as a beta version. 
Demo here : https://koala-test-294100.ts.r.appspot.com/

I really hope you guys like it too and look forward to hearing from you about it.

Peng 


## Part 1 - Puzzle Game

#### Puzzle #1

Answer:

```
doSomething
|-----------------|
                  doSomethingElse(undefined)
                  |------------------|
                                     finalHandler(resultOfDoSomethingElse)
                                     |------------------|
```

#### Puzzle #2

Answer:

```
doSomething
|-----------------|
                  doSomethingElse(undefined)
                  |------------------|
                  finalHandler(undefined)
                  |------------------|
```

#### Puzzle #3

Answer:

```
doSomething
|-----------------|
doSomethingElse(undefined)
|------------------|
                  finalHandler(doSomething result)
                  |------------------|
```

#### Puzzle #4

Answer:

```
doSomething
|-----------------|
                  doSomethingElse(doSomething result)
                  |------------------|
                                     finalHandler(doSomethingElse result)
                                     |------------------|
```

## Quick challenge


### first attempt
The first attemp is using two loops and I think the logic is quite easy to follow.
```

function auditingClasses({students: studentArr}) {
    let results = {}
    studentArr.forEach(item => {
        if(item.class in results) {
            results[item.class].total += item.attended
        } else {
            results[item.class]= {"total": item.attended}
        }
    });

    for(r in results) {
        let count = studentArr.filter(obj => obj.class === r).length
        results[r].average=Math.round(results[r].total/count)
    }
    return results
}
```
### second attempt
I try to write another one with one loop, but I still think if there is a better way to achieve this, I would be so glad if you could show me how.
```

function auditingClasses({students: studentArr}) {
    let result={}
    let count={}
    for(let student of studentArr) {
        if(result[student.class]) {
            result[student.class].total += student.attended
            count[student.class]++
            result[student.class].average = Math.floor(result[student.class].total/count[student.class])
            continue
        }
        result[student.class]= {}
        result[student.class].total = student.attended
        count[student.class] = 1
        result[student.class].average = result[student.class].total
    }
    return result
}
```

# weather-widget














## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
