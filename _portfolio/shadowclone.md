---
title: "Shadowclone"
excerpt: "Thwarting and Detecting DOP Attacks with Stack Layout Randomization and Canary<br/><img src='/images/583overview.png'>"
collection: portfolio
---

Control-flow hijacking attacks have been hard to deploy due to the widespread adoption of control-flow attack defenses such as Control-flow Integrity (CFI). This fact has led to a wide deployment of exploiting non-control data, which are not protected by CFI defenses. Non-control data attacks can be used to corrupt critical data or leak sensitive information. Furthermore, data-oriented programming (DOP) is able to achieve Turing-complete computation capabilities without leaving the control-flow graph of the programs.
In this paper, we present a compile time stack layout randomization scheme- Shadowclone -to thwart and detect DOP attacks effectively. Shadowclone generates randomized clones of vulnerable target functions and randomly selects one copy of clones to execute during runtime. In addition, we also insert compile-time random canaries into stack variables and check its integrity before the function returns. In the evaluation section, we show that our approach can thwart and detect DOP attacks efficiently by limiting attacker's success chance to less than 1%. Shadowclone also has low performance overhead when the program is small and has few function calls.

[Code](https://github.com/shibo-chen/Shadowclone) is avaible on Github.
[Report](../_site/files/shadowclone_report.pdf) and [Slides](../_site/files/shadowclone_report.pdf) also available.