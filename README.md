# CAS706_assignment3_Squeak

Assignment 3
Typed Lambda-calculus interpreter.
Your term language should be based on the same term language as A2. The operational semantics is the same as for A2, and the same as in the textbook.

You should first write down the typing rules for your language. You are allowed, even highly encouraged, to improve the A2 grammar to make your life easier, so that typing rules can be made simpler. Your task is to have your interpreter first do type reconstruction for all inputs before any reduction is done. Decent error messages should be returned for untypable terms. The interpreter should then proceed as in assignment 2.

You should also be providing some test cases for your interpreter - make sure to test higher-order functions as well as cases that do not type (such as old stuck cases from assignment 2) and cases that work but are rejected in your typed language.

You should try to leverage the host system as much as you can for this assignment. This can mean having the type inference algorithm return a typed term, from a different ADT, than the original source. Then your interpreter can be much much simpler (and it should be) because all the 'impossible' cases have been removed already.

In other words, you are writing a type inference routine as the most important part of this assignment. The unification algorithm is thus key.

Bonus: implement an interpreter for your language but using the finally tagless method (in whatever language you want, but this is easiest in Haskell/Scala/Ocaml; more bonus for more languages. Can even be done in Agda/Idris). Note that this is cheating, in the sense that even though it is typed, the types are maintained by the host language, so that there is no need to implement unification (or even a separate typing pass) at all!

--------------------------------------

The same procedure and method to do type inference. I didn’t write comments in codes in details because comments are almost the same as Rust.
Some minor difference includes: TypeEnv is initialed in annotate object, so that it doesn’t need to be passed in annot function. We can inspect any object in squeak and I didn’t find a good way to do pretty print, so that I don’t print the whole solutions set. Still, applyOneType can return any sub term’s type as we do in Rust. I didn’t implement all bool operation terms in squeak because I don’t have enough time. I consider they are very easy to be added into just as translation work from one PL to another, because I have built a standard procedure, what’s more, the key of A3 is procedure of type inference and unification. :) 

env := InterpEnv new.
tyTerm := (Annotate new) annot: term.
cSet := Constraint collect: tyTerm.
solutions := Unifier unify: cSet.
Transcript show: (solutions applyOneType: (tyTerm ty));cr.
Transcript show: (Interp Interpreter: tyTerm Env:env) value;cr.

Example: Take the same test case: \y.\x. x+y

"Variable x"
termVAR := VAR new.
termVAR name:'x'.

"Variable y"
termVAR2 := VAR new.
termVAR2 name:'y'.

"calc x+y"
termCALC2 := CALC new.
termCALC2 op:'+'.
termCALC2 arg1: termVAR.
termCALC2 arg2: termVAR2.

"\y.\x. x+y"
termFUN2 := FUN new.
termFUN2 arg:'x'.
termFUN2 body: termCALC2.
termFUN3 := FUN new.
termFUN3 arg:'y'.
termFUN3 body: termFUN2.

"typed term of \y.\x. x+y"
tyTerm2 := (Annotate new) annot: termFUN3.

"constraints set of \y.\x. x+y"
cSet2 := Constraint collect: tyTerm2.

"solutions set of \y.\x. x+y"
sSet2 := Unifier unify: cSet2.

Then we can inspect the solutions sSet to see all solutions.
In this case type of FUN3 is t2, it is a function takes an int, returnTy is a funtion2. Function2 takes an int and returns an int.

If we need to do both type inference and interpretation. Then we can use below function, which is almost the same as InterpE in Rust. It does type inference firstly and then interpretation.
Interp Interpreter: termAPP.

I didn’t find a good way in squeak to export all codes into files. So I just do copy/paste codes into a txt file: squeak.txt
Snapshot and project file are available as well.
