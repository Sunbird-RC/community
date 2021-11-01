# Understanding Schemas

Each registry can store data as entities. The example registry we setup using
CLI already has two types of entities: `Teacher` and `Student`. To view the
schemas, run the following in the directory you setup the registry:

```sh
$ cat schemas/student.json
$ cat schemas/teacher.json
```

> Or you could view the schemas
> [on Github](https://github.com/gamemaker1/registry-setup-files/tree/main/schemas).

A teacher is represented by 5 fields: `name`, `phoneNumber`, `email`, `subject`
and `school`, out of which `name`, `phoneNumber`, `email` and `school` are
required fields. `subject` can be any one of Math, Hindi, English, History,
Geography, Physics, Chemistry, and Biology.

A student is represented by 4 fields: `name`, `phoneNumber`, `email`, and
`school`, all of which are required fields. When setting `school` field, you are
making a 'claim' (i.e., that the student is from the specified school), which
can be 'attested' (i.e., confirmed to be true) by a teacher from the same
school.

## Next Steps

Now that you know a bit about schemas, jump straight into
[playing with the Registry API](../apis/create.md)!
