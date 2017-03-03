# Referential Transparency
1. 什麼是Referential Transparency  
引述[wiki](https://en.wikipedia.org/wiki/Referential_transparency)上面的定義
> An expression is said to be referentially transparent if it can be replaced with its corresponding value without changing the program's behavior.

看似很抽象的解釋，套在FP的領域裡面，具體一點的意思就是：function的執行就是一個expression，然後function的return value也就是它的corresponding value。也就是說，如果這個function的執行，可以直接用它的return value而取代的話，那麼這個function可以說具有Referential Transparency的特性。

2. 為什麼要稱作為Referential Transparency  
這個名詞似乎出自於探討自然語言的研究，這篇文章給了一個有趣的例子[Why do they call it: Referentially transparent](http://www.nobugs.org/blog/archives/2008/11/12/why-do-they-call-it-referentially-transparent/)
`referentially`指的是你參考的那個東西，可以是很多種東西；`transparent`意思是指doesn’t make a difference
假如微軟重新改寫Excel，並且UI都長的一樣的話，對使用者來說根本感覺不出差異性，這個改寫excel的事情來說就是對使用者來說具有`transparent`的特性

以這篇文章的例子來說：
> [Edinburgh] is bigger than a mouse is true.
> [Scotlands capital] is bigger than a mouse” is still true.
> [The city at 56N 3W] is bigger than a mouse” is also true.

這幾個例子[]內的就是reference, 如果不管reference在怎麼換，結果都對的話，那這幾個句子都具有Referential Transparency的特性

在FP的世界來說，比較著重在探討expression或者function是不是具有Referential Transparency的特性

3. Referential Transparency有什麼好處  
這篇文章[Why-is-referential-transparency-a-good-idea](https://www.quora.com/Why-is-referential-transparency-a-good-idea)裡面有提到幾點
> Without it, the meaning of your program text is hard or just too difficult to determine.

假如你的程式碼具有這個特性的話，那麼我們可以很清楚的知道，每個程式碼執行完的結果會是一個固定可預測的結果，不會有不確定性存在，
那麼我們是不是還沒真正的執行程式之前，就可以知道程式碼會有什麼樣的結果，程式邏輯不就更簡單清晰明瞭了嗎？

更進一步來說
> Compiler to reason about program behavior. This can help in proving correctness, simplifying an algorithm, assisting in modifying code without breaking it, or optimizing code by means of memoization, common subexpression elimination, lazy evaluation, or parallelization.

4. 跟pure function的區別  
從書本上面的定義pure function來說：
> A pure function has the following qualities:
> It depends only on the input provided and not on any hidden or external state that may change during its evaluation or between calls.
> It doesn’t inflict changes beyond their scope, such as modifying a global object or a parameter passed by reference

應該說pure function一定會有Referential Transparency的特性，但是有Referential Transparency特性的不一定是pure function

References:  
1. [Referential transparency](https://en.wikipedia.org/wiki/Referential_transparency)  
2. [What is referential transparency?](http://stackoverflow.com/questions/210835/what-is-referential-transparency)  
3. [Why do they call it: Referentially transparent](http://www.nobugs.org/blog/archives/2008/11/12/why-do-they-call-it-referentially-transparent/)  
4. [Why-is-referential-transparency-a-good-idea](https://www.quora.com/Why-is-referential-transparency-a-good-idea)  
5. [Functional Programming in JavaScript: How to improve your JavaScript programs using functional techniques](https://www.amazon.com/Functional-Programming-JavaScript-functional-techniques/dp/1617292826)