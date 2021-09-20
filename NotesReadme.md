#Guide#

### Install PlantUML Mac###
brew install plantuml


Config
-f /Users/aether/Documents/User/Amd/Workspace/scala-uml/src/main/scala/app/ci 
-o /Users/aether/Documents/User/Amd/ -t

-f /Users/aether/Documents/User/Amd/Workspace/scala-uml/docs/examples/
-o /Users/aether/Documents/User/Amd/Workspace/scala-uml/docs/output/out

/Users/arnold.dajao/Documents/IdeaProjects/arnoldmd181/ScalaUml/docs/output/out

|Command|Functionality|
|-------|-------------|
|-f,--files \<path\>   | Directory of input Scala files|
|-o,--output  \<path\>  | Directory of output class diagram |
|-n,--name \<name\> | name of the output file |
|-t,--textual | states that the output format should be in the `.puml` format of PlantUML class diagrams |
  |-e,--exclude r \<regex\> | Excludes classes or packages from the diagram that match a given regular expression <regex>. The regular expression is matched on the package + name. If you want to delete anything in a package named "org::com::foo" you can use (org::com::foo.*). More options are planned here :) |
|-g,--github \<path to config\> | Processes directories of a github repository stated in a config file. More infos on config files [here](#config) |    



Main Method

```scala 3
package app.ci

object Main extends App {
  try {
    execution()
  } catch {
    case e:Exception => logException(e)
  }

  def execution(): Unit = {
    OParser.parse(parser, args, ParseConfig()) match {
      case Some(conf) =>
        println(conf)
        if(conf.in.nonEmpty){
          val procConf = Config(conf)
          val res = processUmlCol(procConf,tryUmlConstruction(procConf))
          res.orNull
        }
      case None => println("naa")
    }
  }
}
```
### Scala Parser SCOPT
```scala 3
package scopt

import scala.collection.{ Seq => CSeq }

abstract class OParserBuilder[C] {

}
  /** Run the parser, and run the effects.
   */
  def parse[C](parser: OParser[_, C], args: CSeq[String], init: C): Option[C] =
    ORunner.runParser(args, init, parser.toList, psetup0) match {
      case (r, es) => ORunner.runEffects(es, esetup0); r
    }
}
```