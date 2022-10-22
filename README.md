# From Github Issue To MongoDB

Use this action to convert issues into a unified JSON structure (thanks to [@stefanbuck](https://github.com/stefanbuck/github-issue-parser)) and save them in a MongoDB Collection.

## Setup

```yml
- uses: script-0/from-github-issue-to-mongodb@v1.0.0
  id: issue-to-mongodb
  with:
    template-path: .github/ISSUE_TEMPLATE/bug-report.yml # required
  env:
    MONGO_URI: ${{ secrets.SSUE_GITHUB_MONGO_URI }} # required
    MONGO_DB: ${{ secrets.ISSUE_GITHUB_MONGO_DB }} # required
    MONGO_COLLECTION: ${{ secrets.ISSUE_GITHUB_MONGO_COLLECTION }} # required
```

The text to be parsed can be set explicitly using `issue-body` input, otherwise it can be left to use the default value of `${{ github.event.issue.body }}`.

## Example

Given an issue form

```yml
body:
  - type: input
    id: favorite_dish
    attributes:
      label: What's your favorite dish?
    validations:
      required: true

  - type: checkboxes
    id: favorite_color
    attributes:
      label:  What's your preferred color?
      options:
        - label: Red
        - label: Green
        - label: Blue
```

And an issue body

```md
### What's your favorite dish?

Pizza

### What's your preferred color?

- [x] Red
- [ ] Green
- [x] Blue
```

The actions output will be

```json
{
  "favorite_dish": "Pizza",
  "favorite_color": ["Red", "Blue"]
}
```

And this document will be saved in specified Mongo DB Collection.

## Action outputs

- `jsonString` - The entire output ,
- `issueparser_<field_id>` - Access individual values


## More on Github Issue parser ?


Want to learn more about this concept? Check out [@stefanbuck's repo](https://github.com/stefanbuck/github-issue-parser).