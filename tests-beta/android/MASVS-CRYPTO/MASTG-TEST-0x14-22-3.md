---
platform: android
title: Predictable Initialization Vectors
id: MASTG-TEST-0x14-22-3
type: [static, dynamic]
weakness: MASWE-0022
---

## Overview

Several block cipher modes require an [initialization vector (IV)](../../../Document/0x04g-Testing-Cryptography.md#Predictable-Initialization-Vector) as one of the initial input to the cipher.
In general, the IV does not have to be kept secret, but it should not be reused or predictable.

Predictable initialization vectors can allow the attackers to conduct [chosen plaintext attack](https://crypto.stackexchange.com/a/18401), and therefore should be avoided in security sensitive scenarios.

## Steps

1. Run @MASTG-TECH-0014 with a tool such as @MASTG-TOOL-0110 on the app binary, or use @MASTG-TECH-0033 (dynamic analysis) with a tool like @MASTG-TOOL-0001, and identify all uses of classes implementing [AlgorithmParameterSpec](https://developer.android.com/reference/java/security/spec/AlgorithmParameterSpec), such as [IvParameterSpec](https://developer.android.com/reference/javax/crypto/spec/IvParameterSpec), [GCMParameterSpec](https://developer.android.com/reference/javax/crypto/spec/GCMParameterSpec), etc.
1. Track all the posterior uses of the identified objects.

## Observation

The output should contain a list of locations where `AlgorithmParameterSpec` objects were created and used.

## Evaluation

The test case fails if you can find `AlgorithmParameterSpec` objects being created with weak or deterministic random generator and then used to initialize a `Cipher` that is used in a security sensitive scenario.

## References

- [Testing Cryptography](../../../Document/0x04g-Testing-Cryptography.md)