# Avoid system Python

Tim D. Smith, Sep 9 2016

## Introduction

This proposal suggests that Homebrew abandon its long-standing position of favoring system Python for applications whenever possible.

## Motivation

The primary reason to prefer Homebrew's Python over system Python is that system Python links the `ssl` module included in the Python stdlib against the end-of-lifed 0.9.8 branch of OpenSSL.

OpenSSL 0.9.8 does not support TLS 1.1 or 1.2. Despite attacks, there are safe modes of operation for TLS 1.0, but TLS 1.0 is nonetheless being phased out. For example, the PCI (Payment Card Industry) standards have set a [sunset date](https://blog.pcisecuritystandards.org/migrating-from-ssl-and-early-tls) for TLS 1.0 and vendors including CDN Fastly [are responding apace](https://www.fastly.com/blog/update-our-tls-10-and-11-deprecation-plan) by progressively disabling TLS 1.0 traffic.

Applications including mercurial have begun to complain if TLS 1.1+ connections cannot be established. Homebrew responded by installing mercurial [against a Homebrew python by default](https://github.com/Homebrew/homebrew-core/issues/3541); as we move boldly into the future, more applications are likely to exhibit this behavior.

## Mitigations

The popular HTTP client library `requests` will prefer PyOpenSSL to the stdlib ssl module if it is available. The `cryptography` package, upon which PyOpenSSL depends, may be built against a Homebrew OpenSSL. This means that applications which rely on `requests` for all network traffic are not affected if the formula installs the necessary additional dependencies.

Alas: not all TLS traffic is HTTP, and not all applications handle HTTP traffic with `requests`.

## Proposed solution

* Begin using `depends_on :python` with reckless abandon for applications that use python at runtime. For most users, most of the time, this will resolve to a dependency on Homebrew python. Users building from source will, subject to resolution of https://github.com/Homebrew/brew/issues/712, build using the first python in $PATH instead.
* Add audit checks that look for suspicious Python shebangs or virtualenvs.
* Continue to allow implicit dependencies on system Python for library bindings, since those are unlikely to interact with the ssl module.
* Continue to allow implicit build dependencies on system Python, unless network traffic is involved.

Concerns with this approach are:

* If you don't want to install Homebrew's Python, it is inconvenient to globally opt out.
  * A possible mitigation would be to introduce yet another environment variable to force formulas with a :python dependency to build from source.
  * Another possible mitigation is the [`--with-custom-python`](https://github.com/Homebrew/homebrew-core/pull/3548/files#diff-79aaf9c9ba469f17b9f16f996d7e6290R17) switch deployed for Mercurial, which has the desired effect but introduces class-level conditional dependencies, which don't behave well.
* Homebrew uses one fewer system dependency.
* ?


## Alternatives

* Do nothing: since the majority of Homebrew users use the most recently released version of OS X, Homebrew could elect to solve this issue by inaction if the forthcoming Sierra release linked the system python against a more recent TLS library. Unfortunately, the betas released to date continue to build the ssl module against OpenSSL 0.9.8.
* Use `depends_on "python"` instead. This will always resolve to a dependency on Homebrew python. This is more reliable and more obvious; the bottling behavior for :python is surprising if you haven't spent much time with it. It'll also annoy people that don't want to use Homebrew's python.

## Recommendation

Adopt the proposed solution with zero or more mitigations.