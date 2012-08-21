# About

[inovex](http://www.inovex.de/) public maven repository for artifacts not available from other public repositories.

## Usage

To use this repository just add this to your *pom.xml*:

<pre>
&lt;repositories&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&lt;repository&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;id&gt;inovex-releases&lt;/id&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;url&gt;https://github.com/inovex/mvn-repo/raw/master/releases&lt;/url&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&lt;/repository&gt;
&lt;/repositories&gt;
</pre>

See http://cemerick.com/2010/08/24/hosting-maven-repos-on-github/ for detailed
 instructions on how to use a github repository as maven repository.

## Add artifacts

To add artifacts to the repository you must have write access to the repository.
Follow the instructions below to publish artifacts to the repository.

### First clone the maven respository

<pre>
git clone git@github.com:inovex/mvn-repo.git
</pre>

### Alter the *pom.xml* for the project you want to deploy

<pre>
&lt;distributionManagement&gt;
	&lt;repository&gt;
		&lt;id&gt;repo&lt;/id&gt;
		&lt;url&gt;https://github.com/inovex/mvn-repo/raw/master/releases&lt;/url&gt;
	&lt;/repository&gt;
	&lt;snapshotRepository&gt;
		&lt;id&gt;snapshot-repo&lt;/id&gt;
		&lt;url&gt;https://github.com/inovex/mvn-repo/raw/master/snapshots&lt;/url&gt;
	&lt;/snapshotRepository&gt;
&lt;/distributionManagement&gt;
</pre>


You have to enable javadoc and source generation by attaching javadoc and source
plugin to the jar goal.
see also http://maven.apache.org/plugin-developers/cookbook/attach-source-javadoc-artifacts.html

<pre>
&lt;plugins&gt;
&nbsp;&nbsp;&lt;plugin&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&lt;groupId&gt;org.apache.maven.plugins&lt;/groupId&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&lt;artifactId&gt;maven-source-plugin&lt;/artifactId&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&lt;executions&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;execution&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;id&gt;attach-sources&lt;/id&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;goals&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;goal&gt;jar&lt;/goal&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;/goals&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;/execution&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&lt;/executions&gt;
&nbsp;&nbsp;&lt;/plugin&gt;

&nbsp;&nbsp;&lt;plugin&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&lt;groupId&gt;org.apache.maven.plugins&lt;/groupId&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&lt;artifactId&gt;maven-javadoc-plugin&lt;/artifactId&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&lt;executions&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;execution&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;id&gt;attach-javadocs&lt;/id&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;goals&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;goal&gt;jar&lt;/goal&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;/goals&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;/execution&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&lt;/executions&gt;
&nbsp;&nbsp;&lt;/plugin&gt;
&lt;/plugins&gt;
</pre>


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