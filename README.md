# SimpleIndexer

`SimpleIndexer` use `DocumentArrayMemmap` for indexing `Document`. It is recommended to be used in most of the simple use cases when you have less than one million `Document`. 

`SimpleIndexer` leverages `DocumentArrayMmap`'s [`match`](https://docs.jina.ai/api/jina.types.arrays.neural_ops/?jina.types.arrays.neural_ops.DocumentArrayNeuralOpsMixin.match#jina.types.arrays.neural_ops.DocumentArrayNeuralOpsMixin.match) function and searches the `k` nearst neighbors for the query `Document` based on their `embedding` field by the brutal-force search. By default, it calculates the `cosine` distance and returns all the indexed `Document`.



## Advanced Usages

### Configure the index directory

`SimpleIndexer` stores the `Document` at the directory, which is specified by `workspace` field under the [`metas`](https://docs.jina.ai/fundamentals/executor/executor-built-in-features/#meta-attributes) attribute. 

You can find how to override `metas` attributes at [docs.jina.ai](https://docs.jina.ai/fundamentals/flow/add-exec-to-flow/#override-metas-configuration)


### Configure the search behaviors

You can use `match_args` argument to pass arguments to the `match` function as below. 

```python
f =  Flow().add(
     uses=SimpleIndexer,
     uses_with={
         'match_args': {
             'metric': 'euclidean',
             'use_scipy': True,
             'limit': 10}})
```
	
- For more details about overriding [`with`](https://docs.jina.ai/fundamentals/executor/executor-built-in-features/#yaml-interface) configurations, please refer to [here](https://docs.jina.ai/fundamentals/flow/add-exec-to-flow/#override-with-configuration). 
- You can find more about the `match` function at [here](https://docs.jina.ai/api/jina.types.arrays.neural_ops/?jina.types.arrays.neural_ops.DocumentArrayNeuralOpsMixin.match#jina.types.arrays.neural_ops.DocumentArrayNeuralOpsMixin.match)



**At search time**, you can also pass arguments to config the `match` function. This can be useful when users want to query with different arguments for different data requests. For instance, the following codes query with a custom `limit` in `parameters` and only retrieve the top 100 nearest neighbors. 


```python
with f:
    f.search(
        inputs=Document(text='hello'), 
        parameters={'limit': 100})
```

## Used-by

- [Crossmodal Search for ImageNet](https://github.com/jina-ai/example-crossmodal-search)
- [Semantic Wikipedia Search with Transformers](https://github.com/jina-ai/examples/tree/master/wikipedia-sentences)
- [Find Similar Audio Clips](https://github.com/jina-ai/examples/tree/master/audio-to-audio-search)


## Reference
- [Indexers on Jina Hub](https://docs.jina.ai/advanced/experimental/indexers/)