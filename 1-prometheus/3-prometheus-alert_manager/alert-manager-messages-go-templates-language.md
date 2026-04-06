
[Link to Go templates online Interpreter for testing](https://repeatit.io/)

[Go templates quick tutorial](https://developer.hashicorp.com/nomad/tutorials/templates/go-template-syntax)

[JSON <-> YAML Converter](https://www.bairesdev.com/tools/json2yaml/)

>Go templates depend on json or yaml files key and value as input.

>These `{{` are called left delimiter.
>These `}}` are called right delimiter.
>To dereference a key put it between delimiters `{{ .<KEY> }}` => `{{.name}}`
>To dereference a function put it between delimiters `{{ <FUNCTION> }}` => `{{ myFunction }}` without the `.`  => dot. 

---
## **EXP. #1:** Simple key and value dereferencing:

**Input json/yaml:**
```json
{"name": "ahmed", "age": 50}
```

```yaml
name: "ahmed"
age: 50
```

**Go template:**
```go
Hello {{.name}}, you are {{.age}} old!
```

**Output:**
```text
Hello ahmed, you are 50 old!
```

---

## **EXP #2:** Object key and value dereferencing:

**Input json/yaml:**
```json
{"person": {"name": "ahmed", "age": 50} }
```

```yaml
person:
  name: "ahmed"
  age: 50
```

**Go template:**
```go
Hello {{.person.name}}, you are {{.person.age}} old!
```

**Output:**
```text
Hello ahmed, you are 50 old!
```

---
## **EXP. #3:** String manipulation:

**Go template:**
```GO
1.This is a simple template
2.It can {{ "output" }} something.
3.It also
4.{{- " demonstrates" }} trim markers.
5.{{/* it has a comment */}}
6.It can {{ "output" }} something.
7.It can demonstrate {{ "output" | print }} using pipelines.
8.It also {{ $A := "assigns variables" }}{{ $A }}.
9.And conditionals:
10.{{ $B := 2 }}{{ if eq $B 1 }}B is 1{{ else }}B is 2{{ end }}
```

**Output:**
```text
1.This is a simple template
2.It can output something.
3.It also
1. demonstrates trim markers.
5.
6.It can output something.
7.It can demonstrate output using pipelines.
8.It also assigns variables.
9.And conditionals:
10.B is 2
```

---
## **EXP. #4:** If condition flow control:

#### **Simple if condition:**

**Input json/yaml:**
```json
{"flag": true}
```

```yaml
flag: true
```

**Go template:**
```go
{{if .flag}}some text{{end}}
```

**Output:**
```text
some text
```

---
#### **if  else condition:**

**Input json/yaml:**
```json
{"flag": false}
```

```yaml
flag: false
```

**Go template:**
```go
{{if .flag}}some if true text{{else}}some if false text{{end}}
```

**Output:**
```text
some if false text
```

---
#### **if  else if condition:**

**Input json/yaml:**
```json
{"flag": false, "flag_2": true}
```

```yaml
flag: false
flag_2: true
```

**Go template:**
```go
{{if .flag}}some if true text{{else if .flag_2}}some other text{{end}}
```

**Output:**
```text
some other text
```

---
## **EXP. #5:** Range and Loops:

#### 5.1. Print the elements of a list:

**Input json/yaml:**
```json
{"myArray": ["I", "Love", "YOU", "!"]}
```

```yaml
myArray: ["I", "Love", "YOU", "!"]
```

**Go template:**
```go
{{range .myArray}}{{.}}{{end}}
```

**Output:**
```text
ILoveYOU!
```

---
#### 5.2. Print the elements of a list with index:

[Reference video Link](https://www.youtube.com/watch?v=PNBEHBcq5_Q)

**Input json/yaml:**
```json
{"myArray": ["I", "Love", "YOU", "!"]}
```

```yaml
myArray: ["I", "Love", "YOU", "!"]
```

**Go template:**

>Note: the position of $myIndex, $myElement is a place holder for index and the element itself.
```go
{{range $myIndex, $myElement := .myArray}}
{{$myIndex}}: {{$myElement}}.{{end}}
```

**Output:**
```text

0: I.
1: Love.
2: YOU.
3: !.
```

---
#### 5.3. Print a string depending on the number of elements in a list:

**Input json/yaml:**
```json
{"myArray": ["I", "Love", "YOU", "!"]}
```

```yaml
myArray: ["I", "Love", "YOU", "!"]
```

**Go template:**
```go
{{range .myArray}}text {{end}}
```

**Output:**
```text
text text text text 
```

## **EXP. #6:** define multiple templates in one .tmpl file:


#### 6.2. using {{template "myTemplate" .}}:

**Input json/yaml:**
```json
{
"myArray": ["I", "Love", "YOU", "!"],
"human":{"name": "ahmed", "age": 50}
}
```

```yaml
myArray: ["I", "Love", "YOU", "!"]
human:
  name: "ahmed"
  age: 50
```

**Go template:**
> Note: don't forget the dot -> `.` in `{{template "myTemplate" .}}`.
```go
{{template "myTemplate" .}}

{{define "myTemplate"}}

{{range $myIndex, $myElement := .myArray}}
{{$myIndex}}: {{$myElement}}.{{end}}

{{end}}

{{define "myOtherTemplate"}}

Hi {{.human.name}}, You are {{.human.age}}.

{{end}}


```

**Output:**
```text



0: I.
1: Love.
2: YOU.
3: !.








```

---
#### 6.2. useing {{template "myOtherTemplate" .}}:

**Input json/yaml:**
```json
{
"myArray": ["I", "Love", "YOU", "!"],
"human":{"name": "ahmed", "age": 50}
}
```

```yaml
myArray: ["I", "Love", "YOU", "!"]
human:
  name: "ahmed"
  age: 50
```

**Go template:**
```go
{{template "myOtherTemplate" .}}

{{define "myTemplate"}}

{{range $myIndex, $myElement := .myArray}}
{{$myIndex}}: {{$myElement}}.{{end}}

{{end}}

{{define "myOtherTemplate"}}

Hi {{.human.name}}, You are {{.human.age}}.

{{end}}
```

**Output:**
```text


Hi ahmed, You are 50.








```

