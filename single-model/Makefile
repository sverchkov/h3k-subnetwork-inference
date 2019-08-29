# Makefile to run make the inferred networks

# Macros go here
JAR=java -jar
GAMS=gams
PYTHON=python

# Targets start here
.PHONY: subnetwork

# First target to run. The actual pipeline is going to be listed more-or-less in run order.
subnetwork: subnetwork.file

# Establish dirs
input_data output_data gams_intermediates inal_output:
	mkdir $@

# Link files
input_data/ntype2.tab: | input_data # Differential expression
	ln -s ../datasets/ntype2_DE.tab $@

input_data/test_cand_reg.tab: | input_data # Unsure what that is
	ln -s ../datasets/test_cand_reg.tab $@

# Do we need to do something to prepare PPI data? input_data/ppi.tab
# Will we have a tf network? test_tf2.tab
# We probably don't need a literome edge thing: input_data/test_literome.tab

# Candidate paths:
output_data/search_test.gms: SearchTester.jar baseline.config | output_data
	$(JAR) SearchTester.jar baseline.config

# GAMS
#
# Required files:
# solver_target_hits.gms
# max_rna_solver.gms
# combined_solver_max_edges.gms
# cplex.opt

# Run GAMS
gams_intermediates/results.txt: solver_target_hits.gms output_data/search_test.gms cplex.out | gams_intermediates
	$(GAMS) solver_target_hits.gms -lo 2

# Get subnetwork from gams output
subnetwork.file: gams.output
	echo "TODO"



