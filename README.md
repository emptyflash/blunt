# blunt

<img align="right" src="https://cdn.rawgit.com/iOffice/blunt/5cdd7380/blunt_logo.svg" height="150px" style="padding-left: 20px"/>

**blunt** is a matter-of-fact query builder made with [doobie](https://github.com/tpolecat/doobie).
**blunt** provides a simple interface to map database schema to case classes, and an easy to use, typesafe way
of constructing queries. You get all the principled function programming that doobie provides, with an 
additional layer of composability and brevity.

``` scala
import doobie.imports._
import blunt._

val xa = DriverManagerTransactor[IOLite](
  // ...
)

case class Post(id: Int, title: String, subtitle: Option[String], text: String)
case class Comment(id: Int, postId: Int, text: String)

val qb = QueryBuilder[Post]
val post = Post(-1, "A Good Post", None, "Here's a real good post")
qb.insert(post, 'id).build.run.transact(xa).unsafePerformIO

qb.select.join[Comment].on('id, 'postId).build
  .nel
  .transact(xa)
  .unsafePerformIO
// res1: NonEmptyList[(Post, Comment)] = ...
```
