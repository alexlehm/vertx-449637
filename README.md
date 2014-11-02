# issue reproducer for bz449637 (when a farjar is run with a java agent, it unzips the wrong jar giving a NPE)

Small project to show the issue described here https://bugs.eclipse.org/bugs/show_bug.cgi?id=449637 resp. here https://groups.google.com/d/topic/vertx/jOsiGoF-FuU/discussion

When an agent is included in the fatJar invocation, the starter class unzips the wrong jar and gets a NPE, the same probably happens when there are files in the <code>$JAVA_HOME/lib/ext</code> dir. In these cases, the additional jars are after the fatjar in the classpath. OTOH, there may be jars before the farjar in the classpath so we cannot use the first entry (this was the case in the versions up to 2.1.1).

The project contains a mostly empty verticle module and a dummy agent that can be called to show the issue, the pom doesn't build the agent. I have done this manually, but the source file for the agent is in <code>cx.lehmann.agent.DummyAgent</code>.

When running the fatjar with the following command it shows the issue:

<code>java -javaagent:lib\agent.jar -jar target\agentissue-1.0-SNAPSHOT-fat.jar</code>

I assume the best fix would be to iterate through the entries of the classpath and check if it is in fact the fatjar or to resolve the class to the jar and use that.

