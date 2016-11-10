# Homebrew Python application runtime

## Introduction
Package a "secret" keg-only Python distribution to use as a runtime for Python applications in order to reduce the side effects associated with `brew install`ing a Python application.

## Motivation

* HEP 6 described the motivation for avoiding system Python for applications installed by Homebrew and recommended that applications are packaged with `depends_on :python`.
* Adding a PythonRequirement to a formula with `depends_on :python` often has the effect of installing the Homebrew `"python"` package.
* Installing the Homebrew `"python"` package will create a link to a `python` executable in `HOMEBREW_PREFIX/bin`. If Homebrew's bin directory is in PATH and the `"python"` package was not previously installed, this can have the effect of changing the default python interpreter.
* Unintentionally changing the default python interpreter [^default-interpreter] can cause confusion.

[^default-interpreter]: For purposes of this proposal, "the default python interpreter" means "what you get when you run `which python`".

The motivation for this proposal is to establish a way to package Python applications for Homebrew which both

* never uses system Python, and
* never changes the default python interpreter,

and to amend HEP 6 to use this new method.


## Proposed solution

The proposed solution is to add two additional keg-only formulas to homebrew-core, packaging CPython 2.7 and CPython 3. Instead of using `depends_on :python(3)`, all formulas that install Python applications will depend unconditionally on one of these new, keg-only formulas.

The packages installed by these new formulas will be known as the "Homebrew Python application runtime."

## Detailed design

Some desired attributes of the design are:

* The Homebrew Python application runtime (PAR) will be completely independent from the `python` or `python3` formulæ and any other installed Python distributions. Installing the PAR must not change the behavior of any other installed Python distribution. Installing the PAR must not change the default Python interpreter.
* The PAR may never be linked into HOMEBREW_PREFIX.
* Formulas may not install packages to the PAR's prefix.
* The PAR will ship with the virtualenv package, which will subsume the function of the virtualenv `resource` on the `Homebrew::Language::Python::Virtualenv` mixin module.

A present deficiency of this proposal is the absence of a nifty beer metaphor.

A technical concern is the duplication of the Python environment on disk. The PAR will consume ~56 MB. This is considered to be acceptable.

Having multiple Python environments installed is often considered undesirable. However, the PAR distributions will be hidden from the user's environment and will not affect the behavior of any other Python environment, so this is unlikely to contribute to user confusion. A private PAR represents an improvement relative to a dependency on the user-visible Homebrew python.

Another technical concern is that the Apple system python has non-standard behavior with respect to trust stores and the system Keychain; this change will render it impossible to install a Homebrew application which takes advantage of this behavior. This deficiency is considered acceptable because it is not feasible to adopt this behavior in the PAR since it relies on patches to CPython which will not be adopted upstream, and the behavior changes are of niche interest. Further, the trust store behavior is most interesting to corporate environments capable of managing application deployments without Homebrew.

## Alternatives considered

* Make the `python` formula keg-only.


This is not preferred because a) it represents a breaking change and b) the user experience for keg-only formulas can be poor.

> I think people expect to get be able to run python after they run brew install python and I think changing that behavior will break a lot of scripts, invalidate a lot of community knowledge and documentation, and make setting up a development environment harder for new users or inexperienced developers.

[Discussion](https://github.com/Homebrew/brew/pull/1471#issuecomment-259648928)

* Do nothing and continue recommending `--build-from-source` to allow Python applications to build against "foreign" Pythons.

This is not preferred because it is an indirect solution to the actual pain point, which is that changes to the default Python interpreter are undesirable.

Further, the `--build-from-source` workaround relies on the bottle-pouring and dependency-resolution logic for requirements. This logic is subtle, difficult to explain, does not persist through upgrades, and subject to change.

* Automatically add a `--with-custom-python` option to formulas that include the Virtualenv mixin.

This approach attempted to improve the user experience associated with the `:python` requirement. It is not preferred for the same reason as the `--build-from-source` workaround, which is that it is an indirect solution to the actual pain point. [Discussion](https://github.com/Homebrew/brew/pull/1471).


## Footnotes
