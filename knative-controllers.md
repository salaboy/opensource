# Creating a KNative-Style K8s Controller 

This document is intended for developers wanting to create new Controllers following the approach of the [Knative Project](http://knative.dev)

## Project Structure

This section covers the main directories in a controller project. There is no `main.go` file, as the project can be considered a library and the executables are commands (`cmd`) that bootstrap the controllers as we will see in later sections. 

```
cmd/
config/
hack/
pkg/
test/
<vendor/>
go.mod
```

Here is a quick summary of the contents of these directories: 
- **`cmd`**: contains directories with different `main.go` files that boostrap code defined usually inside `pkg/reconciler` 
- **`confif`**: contains yaml files required to install the components into a Kubernetes cluster. These resources uses `ko://` to point to controllers defined in `pkg/reconciler`. Usually to start these components for dev mode you will use `ko -R -f config/`. Inside `config/core` you have the CRDs which your controllers will use. 
- **`hack`**: for me this is where the magic lives. This directory contains a set of scripts that deals with code generation. Look for the code generation section od this document for more insights about these scripts and the things that are being code generated
- **`pkg`**: This is where the code go. A simple controller will have the following subdirectories: `apis`, `client` and `reconciler`: 
  - **`apis`**: a very self-descriptive name for the directory which will contain the types (go structures) that will represent your CRDs and other lifecycle functionality for your resource types. 
  - **`client`**: auto generated code such as `clientset`, `informers`, `injection`, `listers`. 
  - **`reconciler`**: this is where you controllers code will go. In other words, where you will need to write your reconcilation logic. 
- **`test`**: another very self-descriptive directory. Knative components are quite complex, so I will add more details later on about testing and how the tests are organized for `serving` and `eventing`


## Creating a new CRD and Controller

Creating your new CRDs and controllers is a simple process but involves creating different resources and knowing exactly how the code generation will work. 
This section will be showing all the files that are involved in creating new CRDs and their respective controllers code. 

In contrast with other frameworks, Knative choose to not use code generation from the CRDs definitions as these resources doesn't change often and usually are defined just once. 




