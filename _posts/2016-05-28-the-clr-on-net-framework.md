---
inFeed: true
hasPage: true
inNav: false
inLanguage: null
keywords: []
description: 'It is a managed execution platform: there is an execution engine (VM). It takes care of codes execution and provides runtime services '
datePublished: '2016-05-28T10:15:55.328Z'
dateModified: '2016-05-28T10:15:50.389Z'
title: The CLR on .Net Framework
author: []
authors: []
publisher: null
starred: false
sourcePath: _posts/2016-05-28-the-clr-on-net-framework.md
url: the-clr-on-net-framework/index.html
_type: Article

---
# The CLR on .Net Framework

It is a managed execution platform: there is an execution engine (VM). It takes care of codes execution and provides runtime services

The Common Language Infastructure (CLI) defines how the CLR has to be.

The Common Type System (subset of CLI) describes the instruction and the types.

The Common Language (subset of CTS) Specification describes the minimum mandatory set of types.

CLI implementation from MS: CLR, SSCLI (reference implementation of CLI), Compact Framework (CF), .Net MicroFramework, Dynamic Language Runtime (DLR), Core CLR 

Non MS: Mono, DotGNU Portable .NET 

The languages do not have to support 100% what's in the CTS, and they can be wider also. The CTS only defines the minimum set of features.

The CLR is implemented as a set of in process DLLs (they are only loaded into the processes that run managed code). Different application can run on different CLR versions on the same machines. every process has its own runtime specific resources.

_Managed execution key points: _

- the compiler produces an assembly (dll or exe), contining IL and metadata. The assembly cannot execute directly. 

- the process address space is running on top of the machine: when starting an assembly Windows will start a new process address space, and map the assembly image inside the process address space and start the thread of execution. The execution starts in the position specified in the header of the executable file (in metadata): the first set of instructions causes the load bootstrap of the CLR (figuring out if windows is server or client machine, .Net version). Then the CLR does what it takes to perform the initialisation: create more thread, set up the heads, then it loads the assemble into the address space. The entry point is compiled into process specific instructions and then executed, during the execution the required methods are JIT compiled and executed and the resources will be allocated in the heap. 

- there is security like CAS or carnage collector to verify the code is not doing anything not allowed. 

- IL can overload method based on the return type (C\# cannot) 

**JIT Compilation**

* Code is verified to be type-safe, otherwise an exception is raised
* New fields in a type are updated during JIT
* The code is optimised based on the machine it is running (not the developer machine)
* IL is not interpreted
* the default the compilation is on a method by method basis

IL apps

- ndasm: shows IL code 

- ngen.exe improves the performance of managed code 

- fuslogvw: show the assembly binding log viewer. Set the option to create a record for failing assembly bindings 

- sn.exe creates a strong name for the assembly 

- gacutil: adds an assembly to the GAC 

- tlbimp.exe translates COM type info into CLR type info 

- oleview.exe; show the COM components inside an assembly 

- tlbexe.com converts CLR types into CoM types 

- regasm.exe performs COM required registration 

- mscoree.dll builds COM callabe wrappers 

_How to debug IL code_

Activate Debugging for unmanaged code in the project properties. 

Start debugging and open the Immediate window 

Type .load sos 

Type: !name2ee program.exe!ClassName 

Copy the value for the methodName 

Type !dumpmt -md methodName 

Copy the value for the MethodDesc of the desired method 

!u MethodDesc 

**Garbage collection**

Managing memories is difficult to menage for devs (passing refs between assemblies and devs). It's error prone and difficult to debug. 

The CLR provides an heap manager that gives an efficient allocator. 

It dynamics tune the acquisition of the VM based of the memory usage of the program. It grabs larger chunks from the OS and gives to the program than required; release chunks of memories to the OS when they are not required. 

The CLR collects garbage from time to time. No specific operation for that. Periodically the garbage collector checks all the objects and determines which are not references. periodically the GC improves the locality of references and alleviates fragmentation. 

_GC.Collect():_

Use this method to try to reclaim all memory that is inaccessible. It performs a blocking garbage collection of all generations.

_GC.KeepAlive(reference):_

ensure the existence of a reference to an object that is at risk of being prematurely reclaimed by the garbage collector. A common scenario where this might happen is when there are no references to the object in managed code or data, but the object is still in use in unmanaged code such as Win32 APIs, unmanaged DLLs, or methods using COM.

**ASSEMBLIES**

Types are scoped by their assembly (two equals types from different Assemblies are seen as different by the CLR) 

The CLR does assembly resolution: version mapping, name resolution (conversion from the name of the assembly to the code location), validation (CLR takes a look at the found assembly and ensures that it is the right one) and load of the assembly into memory. 

The name of the assembly drives the sophistication of the resolution algorithm. Strong named assembly have a public key. 

Non signed assembly loading: 

- load in the Appbase directory (where the application is hosted) 

- search in a sub of the Appbase with the same name of the assembly (e.g. Appbase/acme/acme.dll) 

- search in additional directory specified in the assembly.exe.config (xcopy deployment)

_Xcopy_: defines additional directories where to look 

Fully specified assembly name include: friendly name, version, culture, key token. 

Search: with a Fully specified assembly name the CLR performs a deeper search 

**Strongly Named Assembly Resolution:**

check if the desired version has been mapped to another version, if the version was already in memory, check if it is in the GAC, check in the CODEBASE hint (specific for the assembly), otherwise fallback on private path probing. Then validate the strong name. 

**Version Policy:**

Map one assembly version to another. Can be done per app/per publisher/per machine. 

Given the originally requested version, via the application policy it can be mapped to another. Then it goes through the publisher policy that can maps to another version and then the machine policy. that can maps to another version. Every policy can replace the previous one. 

The GAC supports side by side deployment of assemblies

**Strong named assembly**

The CLR checks at installation the validity. When it is in the GAC it is not checked again 

**Codebase HINT**

Through the .NET framework configuration it is possible to use a codebase hint, specifying a specific directory where to load the assembly from. If it is a remote path, the assembly gets download locally. 

**Native code cache**

Repository in the end user machine where the assembly can be installed, in order to do it only once. 

Ngen cache is consulted during assembly resolution ![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/7343e273-d5a9-464b-835f-ff71693eebf0.gif)

**Comparing Managed & native Execution**

.NET apps have a mix of managed (CLR based) & native code (Win32/COM) 

_Managed/natived interop is driven by Metadata_

Managed type info can be derived from native type info 

native type info can be derived from managed type info

Tools can help, but sometimes the developers needs to fill the gap 

Two kinds of interop from CLR: with Win32 DLLs, COM components 

Win32 DLLs: carried out by stubs and helpers. type info for Win32 DLLs is lacking (export table contains only names & relative addresses of symbols, cannot know if the symbols are functions or global variables). Cannot get signature or type of functions 

Devs need to provide all the metadata in terms of CLR types. The managed compiler compiles the metadata into the assembly. The JIT compiler uses metadata to build stubs (little chunks of native code emitted by the JIT compiler) the perform the CLR/Win32 transition 

Helpers are native functions that don't need to be tuned specifically to the target (type conversion, assert,..) 

to indicate that a function is in another assembly: 

\[DllImport("ddlName.dll")\] 

static extern type function(parameters) with no body 

**Runtime Callable Wrappers**

manage the gap between CLR and COM 

Takes care of differences in instatiation, type navigation, error handling and memory management 

COM Callabe Wrappers manage the gap between COM and CLR