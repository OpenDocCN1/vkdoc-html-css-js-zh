# 五、利用数组

欢迎学习关于数组和对象的章节。在这一章中，我们继续探索对数组有用的高阶函数。

在我们的 JavaScript 编程世界中，数组随处可见。我们使用它们来存储数据、操作数据、查找数据以及将数据转换(投影)为另一种格式。在这一章中，我们将看到如何使用我们到目前为止所学的函数式编程技术来改进所有这些活动。

我们在 `array` 上创建了几个函数，并从功能上而不是强制性地解决了常见的问题。我们在本章中创建的函数可能已经在数组或对象原型中定义了，也可能没有。请注意，这些是为了理解真正的函数本身是如何工作的，而不是覆盖它们。

### 注意

章节示例和库源代码在第五章分支。回购的网址是 [`github.com/antoaravinth/functional-es8.git`](https://github.com/antoaravinth/functional-es8.git)

一旦你检查出代码，请检查分支第五章:

```
git checkout -b 第五章来源/第五章
```

对于运行代码，与之前一样，运行:

```
npm 跑步游乐场
```

## 函数式处理数组

在本节中，我们将创建一组有用的函数，并使用这些函数解决数组的常见问题。

### 注意

我们在本节中创建的所有函数都被称为*投影函数*。将一个函数应用于一个数组并创建一个新数组或一组新值被称为*投影*。当我们看到第一张投影函数图时，这个术语就有意义了。

### 地图

我们已经看到了如何使用 `forEach` 迭代数组。`forEach` 是一个高阶函数，它将遍历给定的数组，并调用传递的函数，将当前索引作为其参数。`forEach` 隐藏了迭代的常见问题，但是我们不能在所有情况下都使用 `forEach`。

假设我们想对数组中的所有内容求平方，并在一个新的数组中返回结果。我们如何使用 `forEach` 实现这一点？使用 `forEach` 我们无法返回数据；相反，它只是执行传递的函数。这就是我们的第一个投影功能，它被称为地图。

Implementing `map` is an easy and straightforward task given that we have already seen how to implement `forEach` itself. The implementation of `forEach` looks like Listing 5-1.

```javascript
const forEach = (array,fn) => {
    for(const value of arr)
        fn(value)
}
```

Listing 5-1: `forEach` 函数定义

The `map` function implementation looks like Listing 5-2.

```javascript
const map = (array,fn) => {
    let results = []
    for(const value of array)
        results.push(fn(value))
    return results;
}
```

Listing 5-2: 地图功能定义

The `map` implementation looks very similar to `forEach`; it’s just that we are capturing the results in a new array as:

```javascript
let results = []
```

并返回函数的结果。现在是谈论术语*投影函数*的好时机。我们之前提到过地图功能是一个投影功能。为什么我们这样称呼地图函数？原因非常简单明了:因为 `map` 返回给定函数的经过*转换的*值，所以我们称之为投影函数。有些人确实称地图为变换函数，但是我们将坚持使用术语*投影。*

Now let’s solve the problem of squaring the contents of the array using our `map` function defined in Listing 5-2.

```javascript
map([1,2,3], (x) => x * x) =>[1,4,9]
```

As you can see in this code snippet, we have achieved our task with simple elegance. Because we are going to create many functions that are specific to the `Array` type, we are going to wrap all the functions into a `const` called `arrayUtils` and then export `arrayUtils`. It typically looks like Listing 5-3.

```javascript
//map function from Listing 5-2
const map = (array,fn) => {
    let results = []
    for(const value of array)
        results.push(fn(value))
    return results;
}

const arrayUtils = {
    map : map
}

export {arrayUtils}

//another file
import arrayUtils from 'lib'
arrayUtils.map //use map
//or
const map = arrayUtils.map //so that we can call them map
```

Listing 5-3: 将函数包装到 `arrayUtils` 对象中

### 注意

为了清楚起见，在本文中我们称它们为 `map` 而不是 `arrayUtils.map`。

Perfect. To make the chapter examples more realistic, we are going to build an array of objects, which looks like Listing 5-4.

```javascript
let apressBooks = [
    {
        "id": 111,
        "title": "C# 6.0",
        "author": "ANDREW TROELSEN",
        "rating": [4.7],
        "reviews": [{good : 4 , excellent : 12}]
    },
    {
        "id": 222,
        "title": "Efficient Learning Machines",
        "author": "Rahul Khanna",
        "rating": [4.5],
        "reviews": []
    },
    {
        "id": 333,
        "title": "Pro AngularJS",
        "author": "Adam Freeman",
        "rating": [4.0],
        "reviews": []
    },
    {
        "id": 444,
        "title": "Pro ASP.NET",
        "author": "Adam Freeman",
        "rating": [4.2],
        "reviews": [{good : 14 , excellent : 12}]
    }
];
```

Listing 5-4: 描述书籍详细信息的 `pressBooks` 对象

### 注意

这个数组确实包含了由出版社出版的真实书目，但是评论键值是我自己的解释。

我们将在本章中创建的所有函数都将为给定的对象数组运行。现在假设我们需要得到一个只有标题和作者姓名的对象的数组。我们如何使用地图功能达到同样的效果呢？你想到解决办法了吗？

The solution is simple using the `map` function, which looks like this:
```javascript
map(apressBooks, (book) => {
    return {title: book.title, author: book.author}
})
```
That code is going to return the result as you would expect. The object in the returned array will have only two properties: One is `title` and the other one is `author`, as you specified in your function:
```javascript
[
    { title: 'C# 6.0', author: 'ANDREW TROELSEN' },
    { title: 'Efficient Learning Machines', author: 'Rahul Khanna' },
    { title: 'Pro AngularJS', author: 'Adam Freeman' },
    { title: 'Pro ASP.NET', author: 'Adam Freeman' }
]
```

我们并不总是想把所有的数组内容转换成一个新的。相反，我们希望过滤数组的内容，然后执行转换。现在是时候来看看队列中的下一个函数了，过滤器。

### 过滤器

假设我们想要获得评分高于 4.5 的书籍列表。我们将如何实现这一目标？这肯定不是 `map` 要解决的问题，但是我们需要一个类似于 `map` 的函数，它只是在将结果推入结果数组之前检查一个条件。

Let’s first take another look at the `map` function (from Listing 5-2):
```javascript
const map = (array, fn) => {
    let results = []
    for (const value of array)
        results.push(fn(value))
    return results;
}
```
Here we need to check a condition or predicate before we do this:
```javascript
// ...
results.push(fn(value))
// ...
```
Let’s add that into a separate function called `filter` as shown in Listing 5-5.
```javascript
const filter = (array, fn) => {
    let results = []
    for (const value of array)
        (fn(value)) ? results.push(value) : undefined
    return results;
}
```
Listing 5-5

过滤函数定义

With the `filter` function in place, we can solve our problem at hand in the following way:
```javascript
filter(apressBooks, (book) => book.rating[0] > 4.5)
```
which is going to return the expected result:
```javascript
[
    {
        id: 111,
        title: 'C# 6.0',
        author: 'ANDREW TROELSEN',
        rating: [ 4.7 ],
        reviews: [ [Object] ]
    }
]
```

我们一直在改进使用这些高阶函数处理数组的方法。在我们进一步研究数组中的下一个函数之前，我们将看看如何链接投影函数(`map`，`filter`)以在复杂的情况下获得所需的结果。

## 链接操作

It’s always the case that we need to *chain* several functions to achieve our goal. For example, imagine the problem of getting the `title` and `author` objects out of our `apressBooks` array for which the review value is greater than 4.5. The initial step to tackle this problem is to solve it via `map` and `filter`. In that case, the code might look like this:
```javascript
let goodRatingBooks = filter(apressBooks, (book) => book.rating[0] > 4.5)
map(goodRatingBooks, (book) => {
    return {title: book.title, author: book.author}
})
```
which is going to return the result as expected:
```javascript
[
    {
        title: 'C# 6.0',
        author: 'ANDREW TROELSEN'
    }
]
```
An important point to note here is that both `map` and `filter` are projection functions, so they always return data after applying the transformation (via the passed higher order function) on the array. We can therefore chain both `filter` and `map` (the order is very important) to get the task done without the need for additional variables (i.e., `goodRatingBooks`):
```javascript
map(filter(apressBooks, (book) => book.rating[0] > 4.5), (book) => {
    return {title: book.title, author: book.author}
})
```

这段代码实际上告诉了我们正在解决的问题:“映射评级为 4.5 的过滤后的数组，并在一个对象中返回它们的标题和作者键。”由于 `map` 和 `filter` 的性质，我们已经抽象出了数组本身的细节，并开始关注手头的问题。

我们将在接下来的章节中展示链接方法的例子。

### 注意

稍后，我们将看到通过函数组合实现相同功能的另一种方法。

### concatAll

Let’s now tweak the `apressBooks` array a bit, so that we have a data structure that looks like the one shown in Listing 5-6.

```javascript
let apressBooks = [
    {
        name : "beginners",
        bookDetails : [
            {
                "id": 111,
                "title": "C# 6.0",
                "author": "ANDREW TROELSEN",
                "rating": [4.7],
                "reviews": [{good : 4 , excellent : 12}]
            },
            {
                "id": 222,
                "title": "Efficient Learning Machines",
                "author": "Rahul Khanna",
                "rating": [4.5],
                "reviews": []
            }
        ]
    },
    {
        name : "pro",
        bookDetails : [
            {
                "id": 333,
                "title": "Pro AngularJS",
                "author": "Adam Freeman",
                "rating": [4.0],
                "reviews": []
            },
            {
                "id": 444,
                "title": "Pro ASP.NET",
                "author": "Adam Freeman",
                "rating": [4.2],
                "reviews": [{good : 14 , excellent : 12}]
            }
        ]
    }
];
```

Listing 5-6 用书籍详细信息更新了 `apressBooks` 对象

Now let’s take up the same problem that we saw in the previous section: getting the title and author for the books with ratings above 4.5. We can start solving the problem by first mapping over data: `map(apressBooks,(book) => { return book.bookDetails })`

That is going to return us this value:

```javascript
[
    [
        { id: 111, title: 'C# 6.0', author: 'ANDREW TROELSEN', rating: [Object], reviews: [Object] },
        { id: 222, title: 'Efficient Learning Machines', author: 'Rahul Khanna', rating: [Object], reviews: [] }
    ],
    [
        { id: 333, title: 'Pro AngularJS', author: 'Adam Freeman', rating: [Object], reviews: [] },
        { id: 444, title: 'Pro ASP.NET', author: 'Adam Freeman', rating: [Object], reviews: [Object] }
    ]
]
```

如您所见，我们的 `map` 函数返回的数据包含 Array inside Array，因为我们的 `bookDetails` 本身就是一个数组。现在，如果我们将这些数据传递给我们的过滤器，我们将有问题，因为过滤器不能在嵌套数组上工作。

That’s where the `concatAll` function comes in. The job of `concatAll` is simple enough: It needs to concatenate all the nested arrays into a single array. You can also call `concatAll` as a `flatten` method. The implementation of `concatAll` looks like Listing 5-7.

```javascript
const concatAll = (array,fn) => {
    let results = []
    for(const value of array)
        results.push.apply(results, value);
    return results;
}
```

Listing 5-7 `concatAll` 函数定义

在这里，我们只是在迭代结果数组时向上推内部数组。

### 注意

我们已经使用 JavaScript 函数的 `apply` 方法将推送上下文设置为结果本身，并将参数作为迭代值的当前索引进行传递。

The main goal of `concatAll` is to unnest the nested arrays into a single array. The following code explains the concept in action:
```javascript
concatAll(
    map(apressBooks, (book) => {
        return book.bookDetails
    })
)
```
That is going to return us the result we expected:
```javascript
[
  { id: 111, title: 'C# 6.0', author: 'ANDREW TROELSEN', rating: [4.7], reviews: [[Object]] },
  { id: 222, title: 'Efficient Learning Machines', author: 'Rahul Khanna', rating: [4.5], reviews: [] },
  { id: 333, title: 'Pro AngularJS', author: 'Adam Freeman', rating: [4], reviews: [] },
  { id: 444, title: 'Pro ASP.NET', author: 'Adam Freeman', rating: [4.2], reviews: [[Object]] }
]
```
Now we can go ahead and easily do a `filter` with our condition like this:
```javascript
let goodRatingCriteria = (book) => book.rating[0] > 4.5;
filter(
    concatAll(
        map(apressBooks, (book) => {
            return book.bookDetails
        })
    ),
    goodRatingCriteria
)
```
That is going to return the expected value:
```javascript
[
  { id: 111, title: 'C# 6.0', author: 'ANDREW TROELSEN', rating: [4.7], reviews: [[Object]] }
]
```

我们已经看到了在数组世界中设计一个高阶函数是如何优雅地解决许多问题的。到目前为止，我们做得非常好。在接下来的章节中，我们还会看到更多关于数组的函数。

## 缩减功能

如果你在任何地方谈论函数式编程，你经常会听到术语 *reduce functions* 。它们是什么？为什么它们如此有用？`reduce` 是一个漂亮的函数，旨在展示 JavaScript 中闭包的强大功能。在本节中，我们将探讨减少数组的用处。

## 减少功能

To give a solid example of the `reduce` function and where it’s been used, let’s look at the problem of finding the summation of the given array. To start, suppose we have an array called:
```javascript
let useless = [2, 5, 6, 1, 10]
```
We need to find the sum of the given array, but how we can achieve that? A simple solution would be the following:
```javascript
let result = 0;
forEach(useless, (value) => {
    result = result + value;
})
console.log(result) // => 24
```
对于这个问题，我们将数组(包含几个数据)缩减为一个值。我们从简单的*累加器*开始；在这种情况下，我们调用它作为结果来存储我们的求和结果，同时遍历数组本身。请注意，在求和的情况下，我们将结果值设置为默认值 `0`。如果我们需要找到给定数组中所有元素的乘积呢？在这种情况下，我们将把结果值设置为 `1`。设置累加器并遍历数组(记住累加器的前一个值)以产生单个元素的整个过程称为缩减数组。

Because we are going to repeat this process for all array-reducing operations, can’t we abstract these into a function? You can, and that’s where the `reduce` function comes in. The implementation of the `reduce` function looks like Listing 5-8.
```javascript
const reduce = (array, fn) => {
    let accumlator = 0;
    for (const value of array)
        accumlator = fn(accumlator, value)
    return [accumlator]
}
```
**Listing 5-8** 缩减功能优先实现

Now with the `reduce` function in place, we can solve our summation problem using it like this:
```javascript
reduce(useless, (acc, val) => acc + val) // => [24]
```
That is great, but what if we want to find a product of the given array? The `reduce` function is going to fail, mainly because we are using an accumulator value to `0`. So, our product result will be `0`, too:
```javascript
reduce(useless, (acc, val) => acc * val) // => [0]
```
We can solve this by rewriting the `reduce` function from Listing 5-8 such that it takes an argument for setting up the initial value for the accumulator. Let’s do this right away in Listing 5-9.
```javascript
const reduce = (array, fn, initialValue) => {
    let accumlator;
    if (initialValue != undefined)
        accumlator = initialValue;
    else
        accumlator = array[0];
    if (initialValue === undefined)
        for (let i = 1; i < array.length; i++)
            accumlator = fn(accumlator, array[i])
    else
        for (const value of array)
            accumlator = fn(accumlator, value)
    return [accumlator]
}
```
**Listing 5-9** 缩减功能最终实现

我们对 `reduce` 函数进行了修改，现在如果 `initialValue` 没有被传递，`reduce` 函数将把数组中的第一个元素作为其累加器值。

### 注意

看看这两个 `for` 循环语句。当 `initialValue` 未定义时，我们需要从第二个元素开始循环数组，因为累加器的第一个值将被用作初始值。如果调用方传递了 `initialValue`，那么我们需要迭代整个数组。

## 使用 `reduce` 函数解决产品问题

现在，让我们尝试使用 `reduce` 函数来解决我们的产品问题：`reduce(useless, (acc, val) => acc * val, 1) => [600]`。

接下来，我们将在我们的运行示例 `apressBooks` 中使用 `reduce`。为了方便参考，我们将（在清单 5-6 中更新过的）`apressBooks` 引入此处：

```javascript
let apressBooks = [
    {
        name: "beginners",
        bookDetails: [
            {
                "id": 111,
                "title": "C# 6.0",
                "author": "ANDREW TROELSEN",
                "rating": [4.7],
                "reviews": [{ good: 4, excellent: 12 }]
            },
            {
                "id": 222,
                "title": "Efficient Learning Machines",
                "author": "Rahul Khanna",
                "rating": [4.5],
                "reviews": []
            }
        ]
    },
    {
        name: "pro",
        bookDetails: [
            {
                "id": 333,
                "title": "Pro AngularJS",
                "author": "Adam Freeman",
                "rating": [4.0],
                "reviews": []
            },
            {
                "id": 444,
                "title": "Pro ASP.NET",
                "author": "Adam Freeman",
                "rating": [4.2],
                "reviews": [{ good: 14, excellent: 12 }]
            }
        ]
    }
];
```

在一个好日子里，你的老板来到你的办公桌前，要求你实现从我们的 `apressBooks` 中找出好评和优秀评论数量的逻辑。你认为这是一个完美的问题，可以很容易地通过 `reduce` 函数来解决。请记住，`apressBooks` 包含一个数组中的数组（正如我们在上一节中看到的），所以我们需要使用 `concatAll` 来将其展平为一个一维数组。因为评论是 `bookDetails` 的一部分，我们不命名一个键，所以我们可以像下面这样映射 `bookDetails` 并使用 `concatAll`：

```javascript
concatAll(
    map(apressBooks, (book) => {
        return book.bookDetails
    })
)
```

现在，让我们使用 `reduce` 来解决我们的问题：

```javascript
let bookDetails = concatAll(
    map(apressBooks, (book) => {
        return book.bookDetails
    })
)

reduce(bookDetails, (acc, bookDetail) => {
    let goodReviews = bookDetail.reviews[0] != undefined ? bookDetail.reviews[0].good : 0
    let excellentReviews = bookDetail.reviews[0] != undefined ? bookDetail.reviews[0].excellent : 0
    return {
        good: acc.good + goodReviews,
        excellent: acc.excellent + excellentReviews
    }
}, { good: 0, excellent: 0 })
```

这将返回以下结果：

```javascript
[ { good: 18, excellent: 24 } ]
```

现在，让我们逐步分析 `reduce` 函数，看看这个魔法是如何发生的。首先需要注意的是，我们传递了一个累加器作为 `initialValue`，它就是：

```javascript
{ good: 0, excellent: 0 }
```

在 `reduce` 函数体内，我们从 `bookDetail` 对象中获取好评和优秀评论的详情，并将它们存储在相应的变量 `goodReviews` 和 `excellentReviews` 中：

```javascript
let goodReviews = bookDetail.reviews[0] != undefined ? bookDetail.reviews[0].good : 0
let excellentReviews = bookDetail.reviews[0] != undefined ? bookDetail.reviews[0].excellent : 0
```

有了这些，我们可以通过 `reduce` 函数的调用跟踪来更好地理解发生了什么。

对于第一次迭代，`goodReviews` 和 `excellentReviews` 将是：

```javascript
goodReviews = 4
excellentReviews = 12
```

我们的累加器将是：

```javascript
{ good: 0, excellent: 0 }
```

因为我们传递了初始值。

一旦 `reduce` 函数执行了这一行：

```javascript
return { good: acc.good + goodReviews, excellent: acc.excellent + excellentReviews }
```

我们的内部累加器值就变为了：

```javascript
{ good: 4, excellent: 12 }
```

现在我们完成了数组的第一次迭代。在第二次和第三次迭代中，我们没有评论；因此，`goodReviews` 和 `excellentReviews` 都将是 0，但这不会影响我们的累加器值，它保持不变：

```javascript
{ good: 4, excellent: 12 }
```

在我们的第四次也是最后一次迭代中，`goodReviews` 和 `excellentReviews` 将是：

```javascript
goodReviews = 14
excellentReviews = 12
```

累加器值为：

```javascript
{ good: 4, excellent: 12 }
```

现在，当我们执行这一行：

```javascript
return { good: acc.good + goodReviews, excellent: acc.excellent + excellentReviews }
```

我们的累加器值变为：

```javascript
{ good: 18, excellent: 28 }
```

因为我们已经完成了所有数组内容的迭代，所以将返回最新的累加器值，这就是结果。

正如您在这里看到的，在这个过程中，我们将内部细节抽象成了高阶函数，从而产生了优雅的代码。在我们结束本章之前，让我们实现 `zip` 函数，这是另一个有用的函数。

## 拉链阵列

Life is not always as easy as you think. We had reviews within our `bookDetails` in our `apressBooks` details such that we could easily work with it. However, if data like `apressBooks` does come from the server, they do return data like reviews as a separate array, rather than the embedded data, which will look like Listing 5-10.

```
let apressBooks = [
    {
        name : "beginners",
        bookDetails : [
            {
                "id": 111,
                "title": "C# 6.0",
                "author": "ANDREW TROELSEN",
                "rating": [4.7]
            },
            {
                "id": 222,
                "title": "Efficient Learning Machines",
                "author": "Rahul Khanna",
                "rating": [4.5],
                "reviews": []
            }
        ]
    },
    {
        name : "pro",
        bookDetails : [
            {
                "id": 333,
                "title": "Pro AngularJS",
                "author": "Adam Freeman",
                "rating": [4.0],
                "reviews": []
            },
            {
                "id": 444,
                "title": "Pro ASP.NET",
                "author": "Adam Freeman",
                "rating": [4.2]
            }
        ]
    }
];
```

Listing 5-10

拆分 `apressBooks` 对象

```
let reviewDetails = [
    {
        "id": 111,
        "reviews": [{good : 4 , excellent : 12}]
    },
    {
        "id" : 222,
        "reviews" : []
    },
    {
        "id" : 333,
        "reviews" : []
    },
    {
        "id" : 444,
        "reviews": [{good : 14 , excellent : 12}]
    }
];
```

Listing 5-11

`reviewDetails` 对象包含图书的评论详细信息

在清单 5-11 中，评论被填充到一个单独的数组中；它们与图书 `id` 相匹配。这是数据如何被分成不同部分的典型例子。我们如何处理这些分裂的数据？

### zip 功能

`zip` 函数的任务是合并两个给定的数组。在我们的示例中，我们需要将 `apressBooks` 和 `reviewDetails` 合并到一个数组中，这样我们在一棵树下就有了所有必要的数据。

The implementation of `zip` looks like Listing 5-12.

```
const zip = (leftArr,rightArr,fn) => {
    let index, results = [];
    for(index = 0;index < Math.min(leftArr.length, rightArr.length);index++)
        results.push(fn(leftArr[index],rightArr[index]));
    return results;
}
```

Listing 5-12

`zip` 函数定义

`zip` is a very simple function; we just iterate over the two given arrays. Because here we are dealing with two array details, we get the minimum length of the given two arrays using `Math.min`:

```
Math.min(leftArr.length, rightArr.length)
```

一旦获得了最小长度，我们就用当前的 `leftArr` 值和 `rightArr` 值调用我们传递的高阶函数 `fn`。

Suppose we want to add the two contents of the array; we can do so via `zip` like the following: `zip([1,2,3],[4,5,6],(x,y) => x+y) => [5,7,9]`

Now let’s solve the same problem that we have solved in the previous section: Find the total count of good and excellent reviews for the Apress collection. Because the data are split into two different structures, we are going to use `zip` to solve our current problem:

```javascript
//same as before get the //bookDetails
let bookDetails = concatAll(
    map(apressBooks,(book) => {
        return book.bookDetails
    })
)
//zip the results
let mergedBookDetails = zip(bookDetails,reviewDetails,(book,review) => {
  if(book.id === review.id)
  {
    let clone = Object.assign({},book)
    clone.ratings = review
    return clone
  }
})
```

Let’s break down what’s happening in the `zip` function. The result of the `zip` function is nothing but the same old data structure we had, precisely, `mergedBookDetails`:

```json
[
  {
    "id": 111,
    "title": "C# 6.0",
    "author": "ANDREW TROELSEN",
    "rating": [ 4.7 ],
    "ratings": { "id": 111, "reviews": [Object] }
  },
  {
    "id": 222,
    "title": "Efficient Learning Machines",
    "author": "Rahul Khanna",
    "rating": [ 4.5 ],
    "reviews": [],
    "ratings": { "id": 222, "reviews": [] }
  },
  {
    "id": 333,
    "title": "Pro AngularJS",
    "author": "Adam Freeman",
    "rating": [ 4 ],
    "reviews": [],
    "ratings": { "id": 333, "reviews": [] }
  },
  {
    "id": 444,
    "title": "Pro ASP.NET",
    "author": "Adam Freeman",
    "rating": [ 4.2 ],
    "ratings": { "id": 444, "reviews": [Object] }
  }
]
```

The way we have arrived at this result is very simple; while doing the `zip` operation we are taking the `bookDetails` array and `reviewDetails` array. We are checking if both the ids match, and if so we clone a new object out of the `book` and call it `clone`:

```javascript
let clone = Object.assign({},book)
```

现在 `clone` 得到了 `book` 对象中内容的副本。然而，需要注意的重要一点是，`clone` 指向一个单独的引用。添加或操作克隆不会改变真正的图书参考本身。在 JavaScript 中，对象是通过引用来使用的，所以在我们的 `zip` 函数中默认更改 `book` 对象会影响 `bookDetails` 本身的内容，这是我们不想做的。

Once we took up the `clone`, we added to it a `ratings` key with the `review` object as its value: `clone.ratings = review`

最后，我们把它退回去。现在，您可以像以前一样应用 `reduce` 函数来解决这个问题。`zip` 是另一个小而简单的函数，但是它的用途非常强大。

## 摘要

在这一章中，我们已经取得了很大的进步。我们创建了几个有用的函数，比如 `map`、`filter`、`concatAll`、`reduce` 和 `zip`，以便更容易地使用数组。我们将这些函数称为投影函数，因为这些函数总是在应用变换(通过高阶函数传递)后返回数组。需要记住的重要一点是，这些只是高阶函数，我们将在日常任务中使用它们。理解这些功能是如何工作的有助于我们从更多的功能角度来思考，但是我们的功能之旅还没有结束。

本章已经创建了许多有用的数组函数，下一章我们将讨论 currying 和部分应用的概念。这些术语没什么可怕的；它们是简单的概念，但在付诸行动时会变得非常强大。第六章见。