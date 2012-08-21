# About

[inovex](http://www.inovex.de/) public maven repository for artifacts not available from other public repositories.

## Usage

See http://cemerick.com/2010/08/24/hosting-maven-repos-on-github/ for detailed
 instructions on how to use a github repository as maven repository.

### First clone the maven respository

<pre>
git clone git@github.com:inovex/mvn-repo.git
</pre>

### Alter the *pom.xml* for the project you want to deploy

<code>
&lt;distributionManagement&gt;
<br>	&lt;repository&gt;
<br>		&lt;id&gt;repo&lt;/id&gt;
<br>		&lt;url&gt;https://github.com/inovex/mvn-repo/raw/master/releases&lt;/url&gt;
<br>	&lt;/repository&gt;
<br>	&lt;snapshotRepository&gt;
<br>		&lt;id&gt;snapshot-repo&lt;/id&gt;
<br>		&lt;url&gt;https://github.com/inovex/mvn-repo/raw/master/snapshots&lt;/url&gt;
<br>	&lt;/snapshotRepository&gt;
<br>&lt;/distributionManagement&gt;
</code>


You have to enable javadoc and source generation by attaching javadoc and source
plugin to the jar goal.
see also http://maven.apache.org/plugin-developers/cookbook/attach-source-javadoc-artifacts.html

<code>
&lt;plugins&gt;
<br>&nbsp;&nbsp;&lt;plugin&gt;
<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;groupId&gt;org.apache.maven.plugins&lt;/groupId&gt;
<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;artifactId&gt;maven-source-plugin&lt;/artifactId&gt;
<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;executions&gt;
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;execution&gt;
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;id&gt;attach-sources&lt;/id&gt;
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;goals&gt;
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;goal&gt;jar&lt;/goal&gt;
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;/goals&gt;
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;/execution&gt;
<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;/executions&gt;
<br>&nbsp;&nbsp;&lt;/plugin&gt;
<br>
<br>&nbsp;&nbsp;&lt;plugin&gt;
<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;groupId&gt;org.apache.maven.plugins&lt;/groupId&gt;
<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;artifactId&gt;maven-javadoc-plugin&lt;/artifactId&gt;
<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;executions&gt;
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;execution&gt;
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;id&gt;attach-javadocs&lt;/id&gt;
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;goals&gt;
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;goal&gt;jar&lt;/goal&gt;
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;/goals&gt;
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;/execution&gt;
<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;/executions&gt;
<br>&nbsp;&nbsp;&lt;/plugin&gt;
<br>&lt;/plugins&gt;
</code>


### Now deploy the artifacts from the project into the local clone of the maven repository.

The file url is the path to the local maven repository clone.

<pre>
mvn -DaltDeploymentRepository=repo::default::file:../mvn-repo/releases clean deploy
</pre>

### Commit the artifacts added to the repository and push them upstream

<pre>
Rubens-MacBook-Pro:mvn-repo rjenster$ git commit -m "Added artifact: nl.bitwalker:UserAgentUtils:1.6"
Rubens-MacBook-Pro:mvn-repo rjenster$ git push origin master
</pre>
