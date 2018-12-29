---
layout: post
title: GoLang Testing
tags: golang
---

After using Go's basic test framework for a few months, I recently read Go's <a href="https://golang.org/pkg/testing/" target="_blank">official documentation on testing</a>.

A few new things I learned:

- Instead of testing a piece of code, it can be benchmarked with ```BenchmarkXxx(*testing.B)``` in place of ```TestXxx(*testing.T)```.

- You can demonstrate expected behavior of functions with "example"s through ```ExampleXxx()```, then printing out the output of the example, and including on the next line the expected output in a formatted comment (e.g., ```// Output: [-2 0 3 5 100]```). See my implementation of this with <a href="https://github.com/pfreese/go_basics/blob/master/pkg/demo/sort_slice_test.go" target="_blank">ExampleSortedUniqueIntSlice</a>.


