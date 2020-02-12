#### Design

##### Input
| name/directive | type | default value | description |
|------|----|---|-------------|
| default | string \| number | [null] | default value of select element |
| required | boolean | false | add required attribute to input tag |
| options | array | [null] | array object data that is become the options |

##### Process

| name | description
|----|-----|
| setdefaultvalue | set the value to default value if the input **default** exist |
| setvalue | set the value of selected option |
| reset | clear the value of selected option |

##### Output

| name | process | directive v-on: | type | default | description |
|---|---|----|----|----|---|
| value | getvalue() | selected | object, [string \| number] | null, null | get value when the component selected / unselected an option |
|
