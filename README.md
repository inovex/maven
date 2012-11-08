# About

[inovex](http://www.inovex.de/) public maven repository for artifacts not available from other public repositories.

## Usage

To use this repository just add this to your *pom.xml*:

<pre>
&lt;repositories&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&lt;repository&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;id&gt;inovex-releases&lt;/id&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;url&gt;http://inovex.github.com/maven/releases&lt;/url&gt;
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
git clone git@github.com:inovex/maven.git maven-inovex
</pre>

### Alter the *pom.xml* for the project you want to deploy

<pre>
&lt;distributionManagement&gt;
	&lt;repository&gt;
		&lt;id&gt;repo&lt;/id&gt;
		&lt;url&gt;http://inovex.github.com/maven/releases&lt;/url&gt;
	&lt;/repository&gt;
	&lt;snapshotRepository&gt;
		&lt;id&gt;snapshot-repo&lt;/id&gt;
		&lt;url&gt;http://inovex.github.com/maven/snapshots&lt;/url&gt;
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


### Alter ~/.m2/settings.xml to enable the repository for all projects

<pre>
&lt;settings&gt;<br/>&nbsp;&nbsp;&lt;profiles&gt;<br/>&nbsp;&nbsp;&nbsp;&nbsp;&lt;profile&gt;<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;id&gt;inovex-repos&lt;/id&gt;<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;activation&gt;<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;activeByDefault&gt;true&lt;/activeByDefault&gt;<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;/activation&gt;<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;repositories&gt;<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;id&gt;inovex&lt;/id&gt;<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;url&gt;http://inovex.github.com/maven/releases&lt;/url&gt;<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;/repository&gt;<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;/repositories&gt;<br/>&nbsp;&nbsp;&nbsp;&nbsp;&lt;/profile&gt;<br/>&nbsp;&nbsp;&lt;/profiles&gt;<br/>&lt;/settings&gt;
</pre>

### Now deploy the artifacts from the project into the local clone of the maven repository.

The file url is the path to the local maven repository clone.

<pre>
mvn -DaltDeploymentRepository=repo::default::file:../maven-inovex/releases clean deploy
</pre>

### Commit the artifacts added to the repository and push them upstream

<pre>
Rubens-MacBook-Pro:maven-inovex rjenster$ git commit -m "Added artifact: nl.bitwalker:UserAgentUtils:1.6"
Rubens-MacBook-Pro:maven-inovex rjenster$ git push origin master
</pre>