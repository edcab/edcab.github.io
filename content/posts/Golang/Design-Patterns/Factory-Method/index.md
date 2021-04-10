---
title: "Handling objects creating with Factory method"
date: 2021-04-08T08:06:25+06:00
description: 
menu:
  sidebar:
    name: Factory method
    identifier: design-pattern-golang-factory-method
    parent: design-pattern-golang
    weight: 10
hero: images/Golang-logo.png
---

## Factory Method

Is a pattern of type creational that delegates the construction of objects to an interface. The interface has a method that return a type of specific object.

### Problem
The company TransportPassengers has to transport many passengers after your workday.  

### Solution 
First of all, we have to create our factory:

    func GetTransportType(quPassengers int) (product.ITransport, error) {

        if quPassengers >= 1 && quPassengers <=5 {
            return  concrete.NewTaxi(), nil
        }
        if quPassengers >= 6 && quPassengers <=15 {
            return  concrete.NewVan(), nil
        }
        if quPassengers >=16 {
            return  concrete.NewBus(), nil
        }

        return nil, fmt.Errorf("wrong quantity of passengers")
    }

The factory will return the type concrete that you need 

The second step is create an `interface` to define the commons methods of our concrete types of transport.

    type ITransport interface {
        SetName(name string)
        GetName() string
    }


    type Transport struct {
        Name string
    }

    func (g *Transport) SetName(name string) {
        g.Name = name
    }

    func (g *Transport) GetName() string {
        return g.Name
    }

Now we have to create our concrete types (products)

    type bus struct {
        // indirectly implement all methods of iTransport and hence are of iTransport type
        model.Transport 
    }

    func NewBus() product.ITransport {
        return &bus{
            Transport: model.Transport{
                Name: "BUS",
            },
        }
    }

So far we have seen How Can we create the concrete products, factory and interface, but how to glue this parts ?

    func main() {
        typeTransport, _ := factory.GetTransportType(20)
        printDetails(typeTransport)
    }

    func printDetails(g product.ITransport) {
        fmt.Printf("Transport assigned: %s", g.GetName())
        fmt.Println()
    }

### Let's code
GitHub repository: https://github.com/edcab/designpattern-factorymethod-golang.git