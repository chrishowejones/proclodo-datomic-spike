# ldnclj-datomic

## Status

With apologies to Dean the team felt that the server should not
directly expose Datomic queries so that it's easier to wrap security
and validation around the requests.

Currently the client still sends the type of 'query'
i.e. :create-event, :get-events. All 'queries' are sent as POST HTTP
requests.

The concensus is to move towards using a more RESTful approach with
the HTTP method and resource end points to indicate the required data
action.

TODO

   * Add RESTful support for HTTP methods in compjure routes and
   entity end points
   * Add validation - e.g. schema
   * Add security - TBD
   * Example client is broken but I suggest we don't spend time on it
     and just wire in the client from proclodo-reagent-spike and join
     the two as a new project once they are in a stable appropriate point.

### CHJ's thoughts - YMMV

I like the idea of having the only changes to support changes in the
domain model being at the database and the client. However, I believe
that good data validation at the server end point catches a whole
category of bugs and is a major security feature (defence against DoS
and 'injection' style attacks). Therefore I think we need to harden the API on the
server by adding validation.

I also think that supporting a more traditional RESTful interface
makes it easier for new members of the team (and this team is super
fluid!) to grasp the concepts.

_However_ this is not my project. It's a community project. Although I
don't want to be paralysed by constant switching of approaches I will
take arguments to the contrary and am happy to be persuaded
otherwise. For me, the key principle I won't be moved on is
inclusivity. We need to keep the project accessible to as many as possible.

### Dean's thoughts


I was disappointed with how confusing it seemed to get started with Datomic in my first meetup, so I had a think about it and realised that we could greatly simplify the server implementation by hooking Datomic up to core.async in the client.  The example page shows inserting a user, an event, adding the user to the list of atendees, and then using a pull query to get back the data.  It's the pull query that is most interesting - it allows components to ask for exactly the data they need, instead of us having to implement an API to do this.  Changing the domain is now just a matter of changing the Datomic schema and the clojurescript that inserts/displays it.

Things worth mentioning:

* I implemented startup/shutdown using yoyo, thinking that was on the trello board, when in fact it was yada...
* The client is as simple as I could make it so as not to distract - plain cljs/html
* I won't get all wobbly-bottom-lip if we don't use it - this was useful to me on a side project
* There's no security.  EDN-injection protection would be implemented by examining the data passed to insert to see if the current user is allowed to do that, and queries would be filtered for allowed data.  Just an implementation detail...

## Usage

```
$ lein repl
nREPL server started on port 51539 on host 127.0.0.1 - nrepl://127.0.0.1:51539
user=> (require '[meetdown.core])
nil
user=> (in-ns 'meetdown.core)
#object[clojure.lang.Namespace 0x3a6cb9eb "meetdown.core"]
meetdown.core=> (create-dev-system)
#object[meetdown.core$create_dev_system$fn__15391 0x535dd914 "meetdown.core$create_dev_system$fn__15391@535dd914"]
meetdown.core=> (y/start!)
meetdown.core=> (y/stop!) <-- shut down after you're finished
```

Then browse to http://localhost:8090/

## License

Copyright © 2015 p14n

Distributed under the Eclipse Public License either version 1.0 or (at
your option) any later version.
