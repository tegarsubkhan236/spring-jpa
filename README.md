# Accessing JPA Data with REST
This guide walks you through the process of creating an application that accesses relational JPA data through a **hypermedia-based RESTful** front end.

Spring Data REST also supports Spring Data Neo4j, Spring Data Gemfire, and Spring Data MongoDB as backend data stores, but those are not part of this guide.

### Guide Reference
[Accessing JPA Data with REST](https://spring.io/guides/gs/accessing-data-rest)

### API Reference

#### get all links
```bash
curl http://localhost:8080
```
```json
{
  "_links" : {
    "people" : {
      "href" : "http://localhost:8080/people{?page,size,sort}",
      "templated" : true
    }
  }
}
```

#### Create new people
```bash
curl -i -H "Content-Type:application/json" -d '{"firstName": "Frodo", "lastName": "Baggins"}' http://localhost:8080/people
```
```text
HTTP/1.1 201 Created
Server: Apache-Coyote/1.1
Location: http://localhost:8080/people/1
Content-Length: 0
Date: Wed, 26 Feb 2014 20:26:55 GMT
```

#### Get all people
```bash
curl http://localhost:8080/people
```
```json
{
  "_links" : {
    "self" : {
      "href" : "http://localhost:8080/people{?page,size,sort}",
      "templated" : true
    },
    "search" : {
      "href" : "http://localhost:8080/people/search"
    }
  },
  "_embedded" : {
    "people" : [ {
      "firstName" : "Frodo",
      "lastName" : "Baggins",
      "_links" : {
        "self" : {
          "href" : "http://localhost:8080/people/1"
        }
      }
    } ]
  },
  "page" : {
    "size" : 20,
    "totalElements" : 1,
    "totalPages" : 1,
    "number" : 0
  }
}
```

#### Get detail people
```bash
http://localhost:8080/people/1
```
```json
{
  "firstName" : "Frodo",
  "lastName" : "Baggins",
  "_links" : {
    "self" : {
      "href" : "http://localhost:8080/people/1"
    }
  }
}
```

#### Find all the custom queries
```bash
curl http://localhost:8080/people/search
```
```json
{
  "_links" : {
    "findByLastName" : {
      "href" : "http://localhost:8080/people/search/findByLastName{?name}",
      "templated" : true
    }
  }
}
```

#### How to use the findByLastName query
```bash
curl http://localhost:8080/people/search/findByLastName?name=Baggins
```
```json
{
  "_embedded" : {
    "persons" : [ {
      "firstName" : "Frodo",
      "lastName" : "Baggins",
      "_links" : {
        "self" : {
          "href" : "http://localhost:8080/people/1"
        }
      }
    } ]
  }
}
```

#### Edit people
```bash
curl PUT -H "Content-Type:application/json" -d '{"firstName": "Bilbo", "lastName": "Baggins"}' http://localhost:8080/people/1
```
```bash
curl http://localhost:8080/people/1
```
```json
{
  "firstName" : "Bilbo",
  "lastName" : "Baggins",
  "_links" : {
    "self" : {
      "href" : "http://localhost:8080/people/1"
    }
  }
}
```

#### Delete people
```bash
curl -X DELETE http://localhost:8080/people/1
```
```bash
curl http://localhost:8080/people
```
```json
{
  "_links" : {
    "self" : {
      "href" : "http://localhost:8080/people{?page,size,sort}",
      "templated" : true
    },
    "search" : {
      "href" : "http://localhost:8080/people/search"
    }
  },
  "page" : {
    "size" : 20,
    "totalElements" : 0,
    "totalPages" : 0,
    "number" : 0
  }
}
```