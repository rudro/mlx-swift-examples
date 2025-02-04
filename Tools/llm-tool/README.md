# llm-tool

See various READMEs:

- [LLM](../../Libraries/LLM/README.md)

### Building

Build the `llm-tool` scheme in Xcode.

### Running (Xcode)

To run this in Xcode simply press cmd-opt-r to set the scheme arguments.  For example:

```
--model mlx-community/Mistral-7B-v0.1-hf-4bit-mlx
--prompt "swift programming language"
--max-tokens 50
```

Then cmd-r to run.

> Note: you may be prompted for access to your Documents directory -- this is where
the Hugging Face HubApi stores the downloaded files.

The model should be a path in the Hugging Face repository, e.g.:

- `mlx-community/Mistral-7B-v0.1-hf-4bit-mlx`
- `mlx-community/phi-2-hf-4bit-mlx`

See [LLM](../../Libraries/LLM/README.md) for more info.

### Running (Command Line)

`llm-tool` can also be run from the command line if built from Xcode, but 
the `DYLD_FRAMEWORK_PATH` must be set so that the frameworks and bundles can be found:

- [MLX troubleshooting](https://ml-explore.github.io/mlx-swift/MLX/documentation/mlx/troubleshooting)

The easiest way to do this is drag the Products/llm-tool into Terminal to get the path:

```
DYLD_FRAMEWORK_PATH=~/Library/Developer/Xcode/DerivedData/mlx-examples-swift-ceuohnhzsownvsbbleukxoksddja/Build/Products/Debug ~/Library/Developer/Xcode/DerivedData/mlx-examples-swift-ceuohnhzsownvsbbleukxoksddja/Build/Products/Debug/llm-tool --prompt "swift programming language"
```

### Troubleshooting

If the program crashes with a very deep stack trace you may need to build
in Release configuration.  This seems to depend on the size of the model.

There are a couple options:

- build Release
- force the model evaluation to run on the main thread, e.g. using @MainActor
- build `Cmlx` with optimizations by modifying `mlx/Package.swift` and adding `.unsafeOptions(["-O3"]),` around line 87

Building in Release / optimizations will remove a lot of tail calls in the C++ 
layer.  These lead to the stack overflows.

See discussion here: https://github.com/ml-explore/mlx-swift-examples/issues/3
