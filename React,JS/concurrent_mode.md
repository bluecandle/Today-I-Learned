# concurrent mode
<a href ='https://medium.com/swlh/what-is-react-concurrent-mode-46989b5f15da'>link</a>
While being a single person handling 2 different tasks, we were able to make progress on two tasks “at the same time”.

So what is Concurrency? It is a way to structure a program by breaking it into pieces that can be executed independently. This is how we can break the limits of using a single thread, and make our application more efficient.

 On modern machines, to provide the best user experience, we are expected to render 60 frames per second (fps)

To achieve that, for each render cycle our code should run for no more than 16.67 milliseconds, and <b>realistically even less: somewhere around 10 ms</b> (since, as said previously, the browser has also to deal with other UI tasks).

And while most developers will use techniques, such as memoization and debounce, to make the experience feel better, they will just delay the main problem: <b>rendering is still a truck blocking the road.</b>

=> 그래서, time-slicing, interpretive rendering !!
=> 나중에 조금 더 official 되면 살펴봅시다! 재밌긴하네
