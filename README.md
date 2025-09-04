# cyclops_2



/home/sor4003/store_sor4003/wang_wan_et_al_dec2021_molcell_RNAstructure/cyclops/cyclops_2
using DataFrames, Statistics, StatsBase, LinearAlgebra, MultivariateStats, PyPlot, Distributed, Random, CSV, Revise, Distributions, Dates, MultipleTesting
## error ----by revise is not solved 
         ----- u can drop pks that are not necessiary even you can drop any plotting packages 
#####
base_path = "/home/sor4003/store_sor4003/wang_wan_et_al_dec2021_molcell_RNAstructure/cyclops/cyclops_2"

data_path = joinpath(base_path, "data")            # folder where your expression data file is
path_to_cyclops = joinpath(base_path, "CYCLOPS_tumour.jl")  # folder where cyclops.jl lives
output_path = joinpath(base_path, "results")      # folder to write outputs

# load your expression data
expression_data = CSV.read(joinpath(data_path, "expression_matrix.csv"), DataFrame)

seed_genes=["CLOCK","ARNTL","NPAS2","PER1","PER2","PER2","CRY1","CRY2","RORA","RORB","RORC","HLF","DBP","TEF","NR1D1","NR1D2","BHLHE40","BHLHE41","ID1","CIART"] # vector of strings containing gene symbols matching the format of gene symbols in the first column of the expression_data dataframe.

sample_ids_with_collection_times=["R22","R24","R37","R20","R36","R06","R07","R10","R42","R11","R33","R16","R44","R14","R34","R02","R53","R26","R51","R15","R01","R46","R49","R52","R58","R45","R54","R41"] # sample ids for which collection times exist
sample_collection_times = [2.20,2.40,3.40,3.00,3.00,4.00,4.00,4.00,4.00,5.10,5.10,5.12,5.12,5.30,5.30,7.30,7.30,9.30,9.30,14.59,15.55,16.00,18.45,19.23,19.23,19.25,19.25,21.17] # colletion times for sample ids

if ((length(sample_ids_with_collection_times)+length(sample_collection_times))>0) && (length(sample_ids_with_collection_times) != length(sample_collection_times))
    error("ATTENTION REQUIRED! Number of sample ids provided (\'sample_ids_with_collection_times\') must match number of collection times (\'sample_collection_times\').")
end




using Distributed

# Add workers
Distributed.addprocs(length(Sys.cpu_info()))

# Define your paths on all workers
@everywhere begin
    base_path = "/home/sor4003/store_sor4003/wang_wan_et_al_dec2021_molcell_RNAstructure/cyclops/cyclops_2"
    path_to_cyclops = joinpath(base_path, "CYCLOPS.jl")
end

# Include the module on all workers
@everywhere include(path_to_cyclops)

# Load the module in the main process
using .CYCLOPS

