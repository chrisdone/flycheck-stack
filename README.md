# flycheck-stack

A flycheck checker that uses stack ghci

## Requirements

* [stack](http://docs.haskellstack.org/en/stable/install_and_upgrade/)
* [haskell-mode](https://github.com/haskell/haskell-mode)
* [flycheck](https://github.com/flycheck/flycheck)

You can use it with any stack project, or any random file that isn't
in a stack project, in which case it'll use the global configuration.

## Installing

Grab the repo:

```
$ git clone git@github.com:chrisdone/flycheck-stack.git
```

Add the following to your `~/.emacs`:

``` lisp
(add-to-list 'load-path "/path/to/flycheck-stack")
(require 'flycheck-stack)
```

Enable flycheck in your haskell-mode with `M-x flycheck-mode`. Done!

### Other configurations

Enable it for all Haskell files:

``` lisp
(add-hook 'haskell-mode-hook 'haskell-mode-flycheck-stack)
(defun haskell-mode-flycheck-stack ()
  (flycheck-select-checker 'stack)
  (flycheck-mode))
```

Annoyed by the constant checking? Customize the following variable
(removing things you don't like):

``` lisp
(setq flycheck-check-syntax-automatically '(save idle-change new-line mode-enabled))
```

Prefer to trigger it manually?

``` lisp
(define-key haskell-mode-map [f5] 'flycheck-buffer)
```

## Usage

### Basic

When flycheck tries to do a check, the first time for a project you'll
see this in your minibuffer:

    Booting up stack ghci ...

Then in a moment:

    Booted up stack ghci!

Then it's ready for doing checks.

### Switching targets

Some packages have different files and settings for each target
in the .cabal file, and some stack projects have multiple
packages. For this reason stack has a fairly elaborate [means of
specifying targets](http://docs.haskellstack.org/en/stable/build_command/#target-syntax).

Use `M-x flycheck-stack-targets` to set the targets. Examples:

* `M-x flycheck-stack-targets RET my-package1 other-package` -- Load
  up two packages.
* `M-x flycheck-stack-targets RET my-package1:test:my-test-suite` --
  Load up just `my-test-suite` from the `my-package1` package.
* `M-x flycheck-stack-targets RET my-package1:my-test-suite` --
  Load up just `my-test-suite` from the `my-package1` package because
  it's obvious that it's the test suite.
* Finally, `M-x flycheck-stack-targets RET :my-test-suite` --
  Load up just `my-test-suite` from the `my-package1` package because
  it's obvious which one we mean.

### Failure to start up

If the stack ghci fails to start, it'll bring up the buffer with the
output. At this point, you can just run the command in your terminal
to see what the problem is. If it works in the terminal, it should
work in Emacs.
