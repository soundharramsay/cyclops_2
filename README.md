# cyclops_2
/home/sor4003/store_sor4003/wang_wan_et_al_dec2021_molcell_RNAstructure/cyclops/cyclops_2

data_path = joinpath(base_path, "data")            # folder where your expression data file is
path_to_cyclops = joinpath(base_path, "cyclops")  # folder where cyclops.jl lives
output_path = joinpath(base_path, "results")      # folder to write outputs

# load your expression data
expression_data = CSV.read(joinpath(data_path, "your_expression_file.csv"), DataFrame)
