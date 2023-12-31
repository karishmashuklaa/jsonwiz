<h1 align="center">jsonwiz</h1>

<h3 align="center">
  🗷 A command line utility for manipulating JSON files 
</h3>

<div align="center">

[![PyPI version](https://badge.fury.io/py/jsonwiz.svg)](https://badge.fury.io/py/jsonwiz)
[![Downloads](https://static.pepy.tech/personalized-badge/jsonwiz?period=total&units=international_system&left_color=grey&right_color=orange&left_text=Downloads)](https://pepy.tech/project/jsonwiz)
[![Package Status](https://img.shields.io/static/v1?label=status&message=stable&color=brightgreen)](https://pypi.org/project/jsonwiz/)
  
</div>

- [Overview](#overview)
- [Installation](#installation)
- [Usage](#usage)
- [Commands](#commands)
- [Arguments](#arguments)
- [Pass JSON values](#pass-json-values)
- [Pass Arrays](#pass-arrays)
  
## Overview
jsonwiz is a command-line tool for working with JSON files. You can retrieve, modify, add or delete any value in the JSON file, then save the changes back to disk. To access a specific property deep in the tree, you can navigate nested objects and arrays using the dot (.) or slash (/) syntax.

## Installation

jsonwiz can be installed via pip through PyPi

```
pip install jsonwiz
```

## Usage

Once jsonwiz is installed, you can use the following command to interact with the cli

```
jsonwiz COMMAND [FILE] [PATH] [VALUE] [--ARGS]
```

<table>
  <tr>
    <td>COMMAND</td>
     <td>Command to run</td>
  </tr>
  <tr>
    <td>FILE</td>
     <td>Path to JSON file</td>
  </tr>
  <tr>
    <td>PATH</td>
     <td>Key name or path (dot notation or slash notation)</td>
  </tr>
  <tr>
    <td>VALUE</td>
     <td>The new value for `set` command</td>
  </tr>
   <tr>
    <td>ARGS</td>
     <td>Optional arguments to pass. Check below</td>
  </tr>
</table>

## Example

```
jsonwiz set example.json person.name "John" --type string
```
This would write a `name` key into the `person` object, and set the value to `John` as a string

```json
  "person": {
    "name": "John"
  }
```

## Commands

### get
The `get` command fetches an existing value, and outputs it to the console.

```
jsonwiz get example.json person.name
```

### set
The `set` command will create or replace a key. If the key exists, it will override the exisiting value. It also automatically creates any parent objects if necessary.

```
jsonwiz set example.json person.age 32 --type string 
```

The data type of the new value is guessed by the format. To assert required data type, pass the `--type` argument. [Read about it here.](#arguments)

### delete
The `delete` command will delete an existing key, and fail if the key or any parent objects don't exist.

```
jsonwiz delete example.json person.age
```

### validate
The `validate` command simply checks if the input JSON is valid or not.

```
jsonwiz validate example.json
```

## Arguments

### `--type`
The `--type` is an optional argument to specify the data type of the new value. To be used with the `set` command.

Supported types are - `string`, `integer`, `float`, `boolean`, `null` and `object`

Example: Pass a `boolean` value
```
jsonwiz set example.json person.subscribed true --type boolean
```
Output:
```json
{
  "person": {
    "name": "John",
    "subscribed": true
  }
}
```
## Pass JSON values
It is possible to add entire JSON fragments to your file by replacing an existing object/creating a new object. Recommended to use `--type` object to correctly parse your value as JSON when applying to the document. *Make sure to properly escape your quotes when attempting this.*

```
jsonwiz set example.json person.address '{\"city\":\"Mumbai\"}' --type object
```
Output:
```json
  "person": {
    "address": {
      "city": "Mumbai"
    }
  }
```

## Pass Arrays
You can traverse arrays just like any other object, simply by specifying the array index as the key. Make sure to use `--type` object for array insertions to work properly. *Make sure to properly escape your quotes when attempting this.*

<details>
  <summary>Sample JSON</summary>
  
  ```json
  {
  "person": {
    "address": {
      "city": "Mumbai"
    },
    "items": [
      {
        "name": "item1"
      },
      {
        "name": "item2"
      }
    ]
  }
}
  ```
</details>

### get
```
jsonwiz get example.json person.items.0.name
```

### set

1. Create empty array
   
```
jsonwiz get example.json person.jobs [] --type object
```

2. Create arrays with no strings
```
jsonwiz get example.json person.jobs [1,2,3] --type object
```

3. Create arrays with string values
```
jsonwiz my_list '[\"value1\", \"value2\", \"value3\"]' --type object
```
