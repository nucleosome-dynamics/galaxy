Nucleosome Dynamics-Galaxy tools
===================

These wrappers (and the associated datatypes) are part of the Galaxy Tool Shed as ``nucleosome_dynamics`` (and  ``nucleosome_workflow``).

http://toolshed.g2.bx.psu.edu/view/spanish_national_institue_of_bioinformatics/


Nucleosome Dynamics
===================

Nucleosome Dynamics is a suite of R programs for **nucleosome-related analyses** based on MNase-seq experimental data. Each of the analyses included in the toolkit correspond to a Galaxy Tool:

| Galaxy Tool  | Description |
| -------- | -------- |
| readBAM	  | Read Aligned MSase-seq BAM into a RData structure (required for further process) |
| nucleR	   | Determine the positions of the nucleosomes across the genome |
| NFR		  | Short regions depleted of nucleosomes |
| txstart	  | Classify Transciption start accordint to the properties of surrounding nucleosomes	|
| periodicity  | Periodic properties of nucleosomes inside gene bodies |
| stiffness | Aparent stiffness constant foreach nucleosome obtained by fitting the coverage to a gaussian distribution |
| nucleR_stats|	 Nucleosome call statistics|
| nucDyn          | Comparison of two diferent MNase-seq experiments to nucleosome architecture local changes |
| NFR_stats|		 Nucleosome Free Regions statistics|
| txstart_stats|	 TSS and TTS statistics|
| periodicity_stats|  Statistics on Nucleosome periodicity|
| stiffness_stats|	Statistics on stiffness|
| nucDyn_stats|	   Statistics on Nucleosome Dynamics analysis|


Source
======

The code wrapped by Galaxy is a containerized version of 'Nucleosome Dynamics'. The image contains a fully functional 'Nucleosome Dynamics' R installation extended with an entrypoint script that enables a simple and easy way to interact with the set of R packages.

> Source docker image :  mmbirb/nucleosome-dynamics:latest

Visit this link to know more about [Nucleosome Dynamics Docker](https://github.com/nucleosome-dynamics/docker "Nucleosome Dynamics Docker").

Installation
======

### Before Installing
Firstly, you have to [Install docker](https://docs.docker.com/install/ "Install docker"). Once you have docker installed, download the latest image for Nucleosome Dynamics tools from Docker Hub.

``` sh
docker pull mmbirb/nucleosome-dynamics:latest
``` 


Clone the current repository to your installation path:
``` sh
git clone https://github.com/nucleosome-dynamics/nucleosome_dynamics_galaxy.git
cd /nucleosome_dynamics_galaxy
INSTALL_PATH=`pwd`
```

And set where the root directory for your Galaxy installation:
```sh
$GALAXY_ROOT=/your/galaxy/central/path
```


### Install Galaxy Tools

Create a symbolic link or copy Nucleosome Dynamics tools to the tools directory of your local Galaxy instance `$GALAXY_ROOT/tools`:

``` sh
mkdir $GALAXY_ROOT/tools/nucleosome_dynamics
ln -s -t $GALAXY_ROOT/tools/nucleosome_dynamics $INSTALL_PATH/nucleosome_dynamics_galaxy/tools/*
```

Add the following lines to `$GALAXY_ROOT/config/tool_conf.xml`:

``` xml
<section name="Nucleosome Dynamics" id="nucDyn">
    <tool file="nucleosome_dynamics/readBAM.xml" />
    <tool file="nucleosome_dynamics/nucleR.xml" />
    <tool file="nucleosome_dynamics/nucleR_stats.xml" />
    <tool file="nucleosome_dynamics/nucDyn.xml" />
    <tool file="nucleosome_dynamics/nucDyn_stats.xml" />
    <tool file="nucleosome_dynamics/NFR.xml" />
    <tool file="nucleosome_dynamics/NFR_stats.xml" />
    <tool file="nucleosome_dynamics/periodicity.xml" />
    <tool file="nucleosome_dynamics/periodicity_stats.xml" />
    <tool file="nucleosome_dynamics/txstart.xml" />
    <tool file="nucleosome_dynamics/txstart_stats.xml" />
    <tool file="nucleosome_dynamics/stiffness.xml" />
    <tool file="nucleosome_dynamics/stiffness_stats.xml" />
 </section>
```

Restart Galaxy to be sure that the tools are correctly installed by executing:

```sh
$GALAXY_ROOT/run.sh --stop-daemon;
$GALAXY_ROOT/run.sh --daemon;
```


### Add tool data

Copy Nucleosome Dynamics logo to the static directory of your local Galaxy instance `$GALAXY_ROOT/static/images`

``` sh
cp images/NucleosomeDynamicsLogo.png $GALAXY_ROOT/static/images
```

Also add as `tool-data` the gene annotations and the genome size for a list of rellevant reference genomes. As an example, the repository includes the Saccharomyces cerevisiae (version R64-1-1).

##### Create location file
Create a tab separed file  named  `$GALAXY_ROOT/tool-data/nucldyn_publicdata.loc` to point to the actual data location:

``` sh
echo -e "R64-1-1 R64-1-1 Saccharomyces cerevisiae (R64-1-1)\t$INSTALL_PATH/nucleosome_dynamics_galaxy/test-data/refGenomes/R64-1-1/R64-1-1.fa.chrom.sizes\t$INSTALL_PATH/nucleosome_dynamics_galaxy/test-data/refGenomes/R64-1-1/genes.gff" > $GALAXY_ROOT/tool-data/nucldyn_publicdata.loc
``` 

##### Set the location file as a new data table

If necessary, activate the Galaxy data table file:
```sh
cp $GALAXY_ROOT/config/tool_data_table_conf.xml.sample $GALAXY_ROOT/config/tool_data_table_conf.xml
```

Add the following line into `$GALAXY_ROOT/config/tool_data_table_conf.xml`:

```xml
<tables>
    <!-- Locations of chrom size and genes.gff for Nucleosome Dynamics -->
    <table name="nucldyn_publicdata" comment_char="#">
        <columns>value, dbkey, name, pathChromSize, pathGenes</columns>
        <file path="tool-data/nucldyn_publicdata.loc" />
    </table>
```

Restart Galaxy to be sure that the new databable is correctly integratedd by executing:

```sh
$GALAXY_ROOT/run.sh --stop-daemon;
$GALAXY_ROOT/run.sh --daemon;
```

### Import Workflow

You can import Nucleosome Dynamics workflow from the Galaxy web interface.  Got to `[YOUR_GALAXY_URL]/workflows/import` and import it:

1- from URL:  set "Archived Workflow URL" as https://github.com/nucleosome-dynamics/nucleosome_dynamics_galaxy/tree/master/workflow/Galaxy-Workflow-Nucleosome_Dynamics_Workflow.ga"
2- from file: download `workflow/Galaxy-Workflow-Nucleosome_Dynamics_Workflow.ga` and upload it to "Archived Workflow File".

Make sure that all the tools are installed before Importing. You can run the full workflow with this import or create one using create workflow option.


[](./workflow/Galaxy-Workflow-Nucleosome_Dynamics_Workflow.png | width=100)
