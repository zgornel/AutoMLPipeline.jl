# Pipeline
There are three types of Pipelines: `Pipeline`, `ComboPipeline`, and `Selector Pipeline`.
The `Pipeline` (linear pipeline)  performs 
sequential evaluation of `fit_transform!` operation
to each of its elements passing the output of previous element as 
input to the next element iteratively. `ComboPipeline` (feature union pipeline)
performs dataframe concatenation of the final 
outputs of its elements. `Selector Pipeline` acts as
a selector function which outputs the results of the best
learner using its internal cross-validation process.

The `Pipeline` uses `|>` symbolic expression while `ComboPipeline` uses `+`. 
The expression, `a |> b`, is equivalent to `Pipeline(a,b)` function call while
the expression, `a + b`, is equivalent to `ComboPipeline(a,b)`. The
elements `a` and `b` can be transformers, filters, learners or 
pipeline themselves.

### Pipeline Structure
The linear pipeline accepts the following variables wrapped in a 
`Dictionary` type argument:
- `:name` -> alias name for the pipeline
- `:machines` -> a Vector learners/transformers/pipelines
- `:machine_args` -> arguments to elements of the pipeline

For ease of usage, the following function calls are supported:
- `Pipeline(args::Dict)` -> default init function
- `Pipeline(Vector{<:Machine},args::Dict=Dict())` -> for passing vectors of learners/transformers
- `Pipeline(machs::Varargs{Machine})` -> for passing learners/transformers as arguments

### ComboPipeline Structure
`ComboPipeline` or feature union pipeline accepts similar init variables
with the the linear pipeline and follows similar helper functions:
- `ComboPipeline(args::Dict)` -> default init function
- `ComboPipeline(Vector{<:Machine},args::Dict=Dict())` -> for passing vectors of learners/transformers
- `ComboPipeline(machs::Varargs{Machine})` -> for passing learners/transformers as arguments

### Selector Pipeline Structure
`Selector Pipeline` is based on the `BestLearner` ensemble. Detailed explanation can be found in: 
[BestLearner](@ref bestlearner)

### Macro Functions
There are two macro functions available: `@pipeline` and `@pipelinex`. The `@pipeline` macro
is used to process pipeline expression and returns the evaluation of the transformed expression. 
During its recursive parsing, any occurence of `(|>)` is converted to `CombinePipeline`
call and `+` to `Pipeline` calls. To aid in the understanding of the worfklow, `@pipelinex`
shows the transformed expression during the `@pipeline` parsing but just before
the expression is evaluated. The macro `@pipelinex` has similar output to that of `@macroexpand`,
i.e., `pipelinex expr` is equivalent to `@macroexpand @pipeline expr`.

Note: Please refer to the [Pipeline Tutorial](@ref PipelineUsage) for their usage.
