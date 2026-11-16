# BitDust philosophy


## Intro

We are a group of developers working on a prototype for distributed peer-to-peer file storage and private communications software. Our goal is to create a free open source application that everyone can use instead of popular commercial applications such as WhatsApp, Telegram, Dropbox, Google Drive, Facebook, etc.

These applications are very widely used, but they are owned by private companies. Users of these applications do not have any real control over them, or at least do not understand how personal data is processed. On the contrary, the goal of the BitDust project is to create similar applications that are completely open and transparent to everyone and fully owned by people. Potentially, this will completely solve the problem of processing personal data transmitted over the Internet worldwide.

Like the Linux system that helps you control your computer (Linux is completely open and owned by people, unlike Microsoft Windows, which is owned by a private company), the BitDust app helps you control your personal data and also the data that other users share with you. Security, privacy, reliability and respect for user rights are at the core of everything that we create in BitDust.


## The goal

BitDust is a project of a global worldwide distributed computer network for secure data storage, information transfers and independent digital communications. The purpose of BitDust is to make all interactions in the network simple, safe and ethical for each user and thus benefit society as a whole.

In other words, by protecting the privacy of each user, BitDust becomes another way to achieve collective security.


## Personal security

When you are using other nodes for your own purposes, you have to cooperate with them, but must not give them the possibility of fraud (see the trust model in the [Confidence](#confidence) section). Any private information must be encrypted before leaving your device. However, service information, which relates to the infrastructure and operation of the network as a whole, should be open and accessible. This includes the network address published in your identity file - by default the address of your proxy-router, or the address of your own device if routing is disabled, see [Your privacy](identities.md#your-privacy).


## Confidence

The word "trust" in BitDust has a different meaning on each of three levels:

1. __Data - no trust required.__ The security of your data does not depend on trust in other nodes: any private information is encrypted before leaving your device and every packet is digitally signed (see [Personal security](#personal-security) and [How authentication works](identities.md#how-authentication-works)).
2. __Cooperation - trust by default.__ Initially your node is ready to cooperate with other nodes: accept connections, store the data of other users, route traffic.
3. __Reaction to bad behaviour.__ When problems are detected, your node stops cooperating: packets with an invalid digital signature are rejected and unreliable suppliers are automatically replaced (see [Automatic Rebuilding](rebuilding.md)). More advanced methods of protection are planned for the future (see [Software protection](#software-protection)).

By default, all nodes trust each other and expect that the remote user accepts the same rules and that their software follows the same BitDust protocol. By providing the resources of your own device to another user, you trust them initially and expect that their device is running compatible BitDust software. Only in case of unexpected or dangerous behaviour should the software on your device make a negative decision about a particular remote node and block it.

Everyone has the right to modify the BitDust software (see [Control over the program](#control-over-the-program)), but to operate in the common network a modified version must follow the BitDust protocol. This initial trust is not blind: every packet received from another node is verified with its digital signature (see [How authentication works](identities.md#how-authentication-works)), packets with an invalid signature are rejected and unreliable suppliers are replaced (see [Software protection](#software-protection)). In addition, the GNU AGPLv3 license requires anyone who runs a modified version of BitDust in the network to publish its source code - this keeps the network transparent for everyone.

To run your own modified version, clone the software from your own fork of the repository - read more in the [Install BitDust software](install.md) section.


## Software protection

If the software running on your own device finds unpredictable, dubious, dangerous or malicious behavior of other nodes in the network, it must take protective measures against such "attacks" - first of all, stop cooperating with that node (see the trust model in the [Confidence](#confidence) section). Developers can foresee and implement methods of protection against well-known types of "attacks" in the program, but it is impossible to foresee and solve all kinds of possible problems. It will take some time to make the software smart enough to resist most "attacks", but only together can we maximize the security level of the network.


## Self-governance and Independence

Each member of the network should operate as autonomously as possible. A node can support other members of the network and provide them with the resources of its own device, but they must not depend on it entirely. It must be possible at any time, in the normal course of operation, to break the connection with any node and switch to another node automatically. We believe it is not possible to be fully independent in our world, because we share the same environment, but you can make your own digital life as stable and safe as possible and keep it under your control.


## Personal mystery

We believe that the targeted collection and centralization of personal data is extremely dangerous for human society as a whole. In a world where digital information is becoming more and more important, it could lead to discrimination, corruption and fraud, restrict freedom of speech and eventually suppress human individuality. So-called "personal mystery" should always be respected: private information should not be available to any higher authority, even with permission by law.


## "Humanity" behaviour patterns

Nodes in the BitDust network are connected via proxy-routers (by default) or directly and depend on each other, but you make your decisions completely independently. We see many similarities between the structure of the BitDust network and human society in general. BitDust contributors should design and create software based not on abstract mathematical structures and formulas, but on natural human relations. The software must be smart, and we will implement many methods for automating interactions between nodes, but they must be understandable and predictable and ultimately look like normal human behaviour patterns.


## Predictability and controllability

The clearer, more predictable and more controllable the behavior of the software running on your personal computer, the more stable, reliable and transparent you will be to others. All services and features offered by the program must be manageable - each feature individually. There should not be any hidden or unexpected functionality - it must be possible to enable/disable every major feature or service that interacts with other nodes without starting/stopping the program itself.


## Decentralization and distribution

Any centralization in human society, in our understanding, leads to social inequality and divides people. We do not deny and do not enter into a confrontation with the existing order, but look forward to creating a new alternative reality where everyone is equal in rights and opportunities (only in the sense of handling the personal digital information). The BitDust philosophy is simple: decentralized/distributed information flows are always better than any centralized services and self-governance is better than centralized management.


## Forgot your password?

Conventional methods of remote login based on the user's credentials are an evil that must disappear. A local password protects your device, and this is very good practice. However, a password to access a remote server where your personal information is stored puts you in the position of "slave", and the owner of the server becomes your "master" - we must stop that.


## Control over the program

We fully support the position of the GNU community, and Richard Stallman personally: every user must have full control over the software which is running on their device. The BitDust software must always be free and distributed as open source code under a free software license so that anyone can run, study, modify and redistribute it independently.


## Control over the development

We believe that a single team of developers, or even a big IT company, is not able to create some kind of "super multi-purpose" application that meets the needs of all people in the world. Only the community itself, as a number of independent developers operating autonomously, will create a wide variety of applications with a common core running on a generic "engine".

The objective of the BitDust project is not to keep software development under the control of a closed group of people, but rather to create a new foundation and provide support to other people who are doing progressive development of the network and finally influence them in a positive sense.

