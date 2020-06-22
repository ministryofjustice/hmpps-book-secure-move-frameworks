# Person Escort Record framework

This framework contains the sections, steps and questions that need to be completed to produce
a Person Escort Record (PER) when moving somebody using the Book a secure move service.

The framework contains a set of YAML defintions that will be used by the [Book a secure move API](https://github.com/ministryofjustice/hmpps-book-secure-move-api)
and [client applications](https://github.com/ministryofjustice/hmpps-book-secure-move-frontend) of the API to generate validation and form journeys.

## Manifest files

A manifest is a list of sections in the PER defined as a tree-like structure. Each manifest file contains definitions of the steps and the questions that are required to be answered within each section.

Manifest files should live in [`manifests/`](./manifests) and be stored in a flat structure.

### Manifest options

- `name` **(required)** - name of the section. Used for the step heading caption.
- `steps` - a list of steps for this section. See [steps documentation](#steps) for options.

## Steps

Steps define a form page within a particular section. A section can be made up of multiple steps.

### Step options

- `name` **(required)** - the name of the step. Used for the page heading.
- `slug` **(required)** - the URL section for this step
- `questions` **(required)** - list of questions to ask on this step and in what order. Each item should match the filename of the question. See [questions documentation](#questions) for question options.
- `next` - can be used to override the following step or to [define complex step based logic](#next-steps). Value should match the `slug` property.

#### Next steps

The next value of a step can be a relative URL within that journey or an array of conditional steps. Each condition next step can contain a next location, a field name, operator and value.

```yaml
steps:
  -
    name: Step 1
  -
    name: Step 2
    next:
      # an object defining a field and expected value
      -
        field: field1
        value: Yes
        next: conditional-next-step
      # an object defining a field, operator and expected value based on operator
      -
        field: field1
        op: '==='
        value: Yes
        next: conditional-next-step
      # default can be a string
      - default-next-step
```

## Questions

Questions define the way in which to gather a piece of information. Questions can be nested using followup questions on a parent
question and can contain dependency logic to define whether a question should only be answered if another question is answered
in a particular way.

Question files should live in [`questions/`](./questions) and be stored in a flat structure.

### Question options

- `type` **(required)** - the type of question input. Current supported types: `radio`, `checkbox`, `text`, `textarea`
- `question` **(required)** - the full text of the question, usually including a question mark, that should be displayed in forms and summary tables.
- `hint` - hint text to display after the question, usually for advice on how to format an answer
- `options` - for question types that require answers to be within a particular set of items. Usually for radios and checkboxes
  - `label` **(required)** - text displayed on the option label
  - `value` - value submitted to the server, defaults to value of `label`
  - `followup` - list of questions to conditionally display if this question is answered with this value. This will add this question as a dependent question dynamically to any followup questions and include them in the step where this question is defined.
- `validations` - list of [validations](#validations) that need to be applied to this question
  - `name` **(required)** - the validation error key
  - `message` - custom text that will be displayed in the form

#### Validations

Validations are defined as object that contain a validation key (`name`) and a message (`message`) to display if this question fails this validation.

Currently supported validation types:

`required` - mark a question as mandatory. For radios or checkboxes it will require at least one option to be selected, for text or textarea questions it will require at least 1 character
