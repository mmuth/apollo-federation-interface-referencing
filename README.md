# Apollo Federation Referencing Test

This example tries to find a solution to reference an interface of another service in a Federated GraphQL schema.
We have a project service with a `Project` interface and two implementations (`SoftwareDevelopmentProject` and `MarketingProject`) and a rights service, that just holds IDs of accessible projects and tries to reference the projectsIds of the project service.

## Starting the example

Just call

```
yarn install
yarn start-rights-service
yarn start-project-service
yarn start-gateway-service
```

## The goal

The main goal is that we can execute a query like (e.g. finally get the nicename from the Gateway, not only the ID...)

```
query myUser {
  user {
    id
    name
    accessibleProjects {
      id
      niceName
    }
  }
}
```

## Current problem

However it still does not work yet (we only get an array of `null`). The Gateway collects the accessible projects and their specific type in the rights service and then calls the project service with the interface (`...on Project`), but without any representations given, so the projects service does have no chance to deliver anything.

Log-Output:

```
Query:
"query($representations:[_Any!]!){_entities(representations:$representations){...on Project{niceName}}}"


Variables:
{
  "representations": []
}
```

## Other context

Yes, you might notice that we have to know every interface implementation in the rights service. This is also described in [this Github issue](https://github.com/apollographql/apollo-server/issues/2849), but not part of this problem description. If there is a solution to get rid of this fact, that would be even better...

Any input to find a solution here helps ;-) !
