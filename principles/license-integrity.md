# License Integrity

## The Principle

Free software depends on its license. The license is not a formality, it is the legal instrument that gives the project its character and protects its users. For copyleft projects, the license is the mechanism by which the project's freedom is preserved against proprietary capture. An AI tool that introduces incompatible code does not create a minor legal inconvenience. It undermines the project's foundational commitment. In short, copying protected works without attribution violates user protections.

## Copyright as the Basis for Copyleft

Copyleft requires copyright to function. The copyright holder grants use under terms that preserve freedom. Any distribution of the work or derivative works must carry the same terms forward. Without established copyright, there is nothing to license. Without the license, there is no copyleft.

For a project to have copyleft protection:

1. Copyright must be established by adopting a license. This can exist in file headers, a LICENSE file, and optionally a CONTRIBUTORS or AUTHORS file.
1. The license must be stated explicitly
1. Contributions must be made under compatible terms

An AI tool working in a copyleft project participates in this structure. In principle it does not stand outside it. In reality, it operates in a way that may derive copyrighted works without any attribution, violating not only the copyright holder's rights, but violating users rights.

## AI Tool Reality

**Before suggesting a dependency:**
The tool should understand the current license, or attempt to determine whether it is compatible with the project's license. A dependency with an incompatible license is not a neutral suggestion, it would create a violation that may not be discovered until distribution.

**Realistically AI-based tooling when suggesting a dependency:**
Will willingly suggest incompatible dependencies that will create licensing problems that won't immediately surface. It is critically important that [classification](./epistemic-transparency.md#the-classification-requirement) of any suggested library or external tool surfaces during planning, not during code review or later discovery mechanisms.

Compatible pair examples (for illustration only): MIT and Apache 2.0 are compatible with GPLv3. GPLv2-only is not compatible with GPLv3 (the "or later" clause matters). LGPL is compatible with GPL under specific conditions. Proprietary licenses are generally incompatible with copyleft. In no case can code be verbatim copied without proper attribution, as this is simply a copyright violation.

If compatibility is uncertain, tools should flag it explicitly, and not proceed without confirmation.

**In existing files:**
Tools must preserve all copyright notices and license headers. They should not remove them, modify them, or replace them. No "clean up" in a file that strips a copyright header. These notices are legally meaningful.

**When generating code:**
There is no way to know if code introduced reproduces substantial portions of copyleft-licensed code from training data into a project with an incompatible license. Realistically, code should be written by human users with tool assistance, not written by tooling and audited by humans for accuracy. Simply flagging the uncertainty of generated code isn't sufficient compliance with any project's license.

## The "Viral License" Framing

The term "viral" applied to copyleft licenses was promoted by parties with financial interest in discouraging copyleft adoption. It frames a legal mechanism that protects user freedom as a pathogen. This framing is inaccurate and serves proprietary interests.

What copyleft actually does:
- Applies copyright law to preserve freedom rather than restrict it
- Requires that derivative works carry the same terms forward
- Does not spread to independent works that merely coexist in a repository

The ShareAlike requirement in CC BY-SA 4.0 means: if you adapt this specification and distribute your adaptation, your adaptation must also be CC BY-SA 4.0. It does not mean your project's software becomes CC BY-SA. Content licenses and software licenses govern different things. They do not cross-contaminate.

The same principle applies to GPL-family licenses. Copyleft only applies to derivative works of the specific licensed code. Entirely independent programs that exist in the same directory, repository, or operating system are not affected by the presence of a copyleft licensed work.

## This Specification's License

This specification is published under CC BY-SA 4.0. Copyright (c) 2026 Wendall Cada.

The choice is deliberate. A specification that argues for protecting commons resources from corporate capture should protect itself by the same mechanism. A corporation cannot fork this specification, strip the ethics, and distribute a proprietary variant. The ShareAlike requirement prevents it.

This is copyleft functioning as intended.