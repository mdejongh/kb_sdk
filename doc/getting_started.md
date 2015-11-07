# KBase SDK - Building and Registering Your First Module

This guide assumes that you have installed the KBase SDK and its dependencies, including Docker.  If you have not, then follow the [installation guide](installation.md).

In order to follow the registration instructions, you have to be an approved KBase developer.  To become an approved KBase developer, first create a standard KBase user account through http://kbase.us.  Once you have an account, please contact us with your username and we will help with the next steps.

It will also be useful to have an idea of the [components of a module](module_overview.md) and use that guide as a reference when editing your module files.


## 1) Initialize a new module

The KBase SDK provides a way to quickly bootstrap a new module by generating most of the required components.  The basic options of the command are:

    kb-sdk init [-ev] [-l language] [-u username] ModuleName

This command will create a new module with the specified `ModuleName` configured based on the options provided.  You should always provide a username option so that the kbase.yml configuration file for your module includes you as a module owner.  The other key option is to set the programming language that you want to write your implementation in.  You can select either Python, Perl or Java.

The other options are:

    -v, --verbose    Show verbose output about which files and directories
                     are being created.
    -u, --username   Set the KBase username of the first module owner
    -e, --example    Populate your repo with an example module in the language
                     set by -l
    -l, --language   Choose a programming language to base your repo on.
                     Currently, we support Python, Python, and Java. Default is
                     Python

For this guide, let's start by creating a new module to count the number of contigs in a KBase ContigSet object.  This is the same function that is defined in the basic example generated when you run `kbase init` with the `-e` flag, so you can always generate those files and compare.  In this example, we will instead put together the various pieces by hand.

We'll write our implementation in Python, but most steps should apply for all the languages.  Let's start by generating the module directory with the `init` method.  Open a terminal and change to a working directory of your choice.  Start by running:

    kb-sdk init -u [user_name] -l python ContigCount

Module names in KBase need to be unique accross the system.  Since you're not the first one trying out this tutorial, the name ContigCount is probably already taken so you should name it something like MikeContigCount, replacing your name with Mike of course.  Don't worry if your favorite name is taken- there will be ways to provide nicer display names for your module soon.


## 2) Update kbase.yml

Open and edit the kbase.yml file to include a better description of your module.  The default generated description isn't very good.


## 3) Define your KIDL specification

The first step is to define the interface to your code in a KIDL specification.  Open the `ContigCount.spec` file in a text editor, and you will see this:

    /*
    A KBase module: MikeContigCount
    */
    module MikeContigCount {
        /*
        Insert your typespec information here.
        */
    };

Comments are enclosed in `/* comment */`.  In this module, we want to define a function that counts contigs, so let's define that function and its inputs/outputs as:

    typedef string contigset_id;
    typedef structure {
        int contig_count;
    } CountContigsResults;

    funcdef count_contigs(string workspace_name, contigset_id contigset)
                returns (CountContigsResults) authentication required;

There a few things introduced here that are part of the KBase Interface Description Language (KIDL).  First, we use `typedef` to define the structure of input/output parameters using the syntax:

    typedef [type definition] [TypeName]

The type definition can either be another previously defined type name, a primitive type (string, int, float), a container type (list, mapping) or a structure.  In this example we define a string named `contigset_id` and a structure named `CountContigResults` with a single integer field named `contig_count`.

We can use any defined types as input/output parameters to functions.  We define functions using the `funcdef` keyword in this syntax:

    funcdef method_name([input parameter list]]) returns ([output parameter list]);

Optionally, as we have shown in the example, your method can require authentication by adding that declaration at the end of the method.  In general, all your methods will require authentication.

TODO: the rest

## 4) Generate the Python implementation stub

## 5) Implement your method

## 6) Setup and run tests

## 7) Define a Narrative Method Specification

## 8) Add everything to a public git repo

## 9) Register your method thorugh the Narrative

## 10) Run the new Narrative Method