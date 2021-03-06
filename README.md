FileMaker XML custom functions
=========

There have been a number of FileMaker custom functions to work with XML - many of them based on a function produced by Andy Knasinski from NRG Software which has the honour of being custom function number 1 on Brian Dunning's site: [http://www.briandunning.com/cf/1](http://www.briandunning.com/cf/1). Michael Wallace from Richard Carlton Consulting also wrote an initial implementation of an XPath function [http://www.briandunning.com/cf/976](http://www.briandunning.com/cf/976) which uses Andy's function to access data.

Unfortunately both of these functions fell short in work which I was doing which required more sophisticated access and a more complete implementation of XPath, as well as the ability to process XML which contained multiple arguments on almost all nodes, and to find child nodes where attributes matched specific criteria.

As with anything parsing XML or JSON in FileMaker it's still based on text string parsing, which means it's not entirely foolproof and there are still places where unexpected results will occur. 

Hopefully these functions will be useful to others - any suggestions for improvements most welcome.

### XMLXPath

```
XMLXPath ( XML; XPath )
```

This is the core function which you will most likely use. It relies on three of the other custom functions to be present. The function supports the following

- /node - will return the XML of the requested node - note that unlike Andy's function you get the full XML node back, not the content of the node
- /node/child[1] - will return the first child node of the parent node (again, the full XML) to get just the value (assuming no further child nodes) use XMLValue() on the result
- /node/child[1]/@argument - will return the requested argument from the requested node
- /node/child[@argument='something'] - will return all child nodes where the requested argument matches the query (wrapped in single quotes).
 
Essentially the function will recurse through the requested XPath until it locates the specific data required. Note that at this time the XPath selectors are limitied to those described above - if you've got ideas for others to add let us know (pull requests always welcome :-)

### XMLValue

```
XMLValue ( XML )
```

Returns the value content of a node (as opposed to the node itself). Typically this would be called on a node retrieved using `XMLXPath()`

### XMLNodeAttribute

```
XMLNodeAttribute ( XML; node; instance; attribute )
```

Used as a shortcut to retrieve a specific attribute from a specific node. Note that this is simpler to use, but more fragile than using `XMLXpath()` because it will find the requested instance of node in the XML, even if that's not necessarily in the correct place.

### XMLAttribute

```
XMLAttribute ( XML; attribute )
```

Returns the requested attribute from the first node encountered in XML, or null if that argument doesn't exist. This function is required by `XMLXpath()`.

### XMLNode

```
XMLNode ( XML; node; instance)
```

Returns the requested node from within the XML path provided. This function is required by `XMLXpath()`.

### XMLFindNode

```
XMLFindNode (XML; node; query; instance; stack)
```

Recurses through the supplied XML looking for child nodes with name node, which match query. This function is required by `XMLXpath()`.