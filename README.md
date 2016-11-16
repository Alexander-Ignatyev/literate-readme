#!/usr/bin/env stack
> -- stack --install-ghc runghc --package turtle --package markdown-unlit -- "-pgmL markdown-unlit"

```haskell
{-# LANGUAGE OverloadedStrings #-}

import Turtle
```

# Literate README

You can setup and build this project.

```haskell
parser :: Parser (Bool, Bool, Bool)
parser = (,,) <$> switch "setup" 's' "Set up the stack environment."
              <*> switch "test"  's' "Build the project and run the tests."
              <*> switch "build" 'b' "Just build, don't run tests."
```

```haskell
main = void $ do
    (setup, test, build) <- options "Literate README" parser
    let ops = doSetup setup .&&. doBuild build .&&. doTest test
    ops .||. die "Failed."

nop = shell "true" empty
```

## Setup

```haskell
doSetup True = shell "stack setup" empty
doSetup _    = nop
```


## Build

```haskell
doBuild True = shell "stack build" empty
doBuild _    = nop
```


## Test

```haskell
doTest True = shell "stack test" empty
doTest _    = nop
```