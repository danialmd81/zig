import CodeBlock from "@theme/CodeBlock";

import SentinelTerminationPtrcast from "!!raw-loader!./29.sentinel-termination-ptrcast.zig";
import SentinelTerminationStringLiteral from "!!raw-loader!./29.sentinel-termination-string-literal.zig";
import SentinelTerminationCString from "!!raw-loader!./29.sentinel-termination-c-string.zig";
import SentinelTerminationCoercion from "!!raw-loader!./29.sentinel-termination-coercion.zig";
import SentinelTerminationSlicing from "!!raw-loader!./29.sentinel-termination-slicing.zig";

# Sentinel Termination

Arrays, slices and many pointers may be terminated by a value of their child
type. This is known as sentinel termination. These follow the syntax `[N:t]T`,
`[:t]T`, and `[*:t]T`, where `t` is a value of the child type `T`.

An example of a sentinel terminated array. The built-in
[`@ptrCast`](https://ziglang.org/documentation/master/#ptrCast) is used to
perform an unsafe type conversion. This shows us that the last element of the
array is followed by a 0 byte.

<CodeBlock language="zig">{SentinelTerminationPtrcast}</CodeBlock>

The types of string literals is `*const [N:0]u8`, where N is the length of the
string. This allows string literals to coerce to sentinel terminated slices, and
sentinel terminated many pointers. Note: string literals are UTF-8 encoded.

<CodeBlock language="zig">{SentinelTerminationStringLiteral}</CodeBlock>

`[*:0]u8` and `[*:0]const u8` perfectly model C's strings.

<CodeBlock language="zig">{SentinelTerminationCString}</CodeBlock>

Sentinel terminated types coerce to their non-sentinel-terminated counterparts.

<CodeBlock language="zig">{SentinelTerminationCoercion}</CodeBlock>

Sentinel terminated slicing is provided which can be used to create a sentinel
terminated slice with the syntax `x[n..m:t]`, where `t` is the terminator value.
Doing this is an assertion from the programmer that the memory is terminated
where it should be - getting this wrong is detectable illegal behaviour.

<CodeBlock language="zig">{SentinelTerminationSlicing}</CodeBlock>
