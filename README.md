# Here we show a python code for searching articles #

** Biopython module will be used **

# Using the search strategies indicated above, we first check how many of such articles there are:


from Bio import Entrez   # biopython module for searching on Entrez
from Bio import Medline
import numpy as np

Entrez.email = "bewketu.bekele@gmail.com"  

#SearchTerm="((((Human immunodeficiency virus) OR HIV OR AIDS) AND (Malawi OR (South Africa))) AND ('treatment failure' OR dropout))"

SearchTerm= "((((Human immunodeficiency virus) OR (acquired immunodeficiency syndrome) OR HIV OR AIDS OR (HIV/AIDS)) AND (Swaziland OR Botswana OR Lesotho OR (South Africa) OR Zimbabwe OR Namibia OR Zambia OR Mozambique OR Malawi OR Uganda OR (Equatorial Guinea) OR Kenya OR Tanzania OR (sub Saharan Africa) OR (sub-Saharan Africa)))  AND ((treatment failure) OR switch OR switches OR switching OR (first line) OR (first-line) OR (second line) OR (second-line) OR (lost to follow up) OR (lost to follow-up) OR (loss to follow up) OR (loss to follow-up) OR dropout OR (drop-out) OR dropouts OR (drop out) OR (drop outs)))" 

# SearchTerm= "((((Human immunodeficiency virus) OR (acquired immunodeficiency syndrome) OR HIV OR AIDS OR (HIV/AIDS)) AND (Swaziland OR Botswana OR Lesotho OR (South Africa) OR Zimbabwe OR Namibia OR Zambia OR Mozambique OR Malawi OR Uganda OR (Equatorial Guinea) OR Kenya OR Tanzania OR (sub Saharan Africa) OR (sub-Saharan Africa)))  AND ((treatment failure) OR switch OR switches OR switching OR (first line) OR (first-line) OR (second line) OR (second-line) OR (lost to follow up) OR (lost to follow-up) OR (loss to follow up) OR (loss to follow-up) OR dropout OR (drop-out) OR dropouts OR (drop out) OR (drop outs)))  AND  ((2010/01/01[PDat] : 2015/12/31[PDat]) AND Humans[Mesh] AND English[lang]))"

#SearchTerm="((('Human immunodeficiency virus' OR 'acquired immunodeficiency syndrome' OR HIV OR AIDS OR 'HIV/AIDS') AND (Swaziland OR Botswana OR Lesotho OR 'South Africa' OR Zimbabwe OR Namibia OR Zambia OR Mozambique OR Malawi OR Uganda OR 'Equatorial Guinea' OR Kenya OR Tanzania OR 'sub Saharan Africa' OR 'sub-Saharan Africa'))  AND ('treatment failure' OR switch OR switches OR switching OR 'first line' OR 'first-line' OR 'second line' OR 'second-line' OR  'lost to follow up' OR 'lost to follow-up' OR 'loss to follow up' OR 'loss to follow-up' OR dropout OR 'drop-out' OR dropouts OR 'drop out' OR 'drop outs'))"

handle = Entrez.egquery(term=SearchTerm)

record = Entrez.read(handle)
for row in record["eGQueryResult"]:
	if row["DbName"]=="pubmed":
		print row["Count"]		# prints the total number of articles (suppose N) with the same search concepts

# Now we use the Bio.Entrez.efetch function to download the PubMed IDs of these articles:
handle = Entrez.esearch(db="pubmed", term=SearchTerm, retmax=619)  # put N=the maximum number of articles you will like to download

record = Entrez.read(handle)
idlist = record["IdList"]
print idlist

# This returns a python list containing all of the PubMed IDs of articles related to the search concepts. i.e. it will display as [18680603', '18665331', '18661158', '18627489' ... ]

# ===========  ======================= 
# Now that we've got the PubMed IDs, 
# The following commands will give us the corresponding Medline records and extract the information from them. Here, we'll download the Medline records in the Medline flat-file format, and use the Bio.Medline module to parse them:

handle = Entrez.efetch(db="pubmed", id=idlist, rettype="medline",retmode="text")
records = Medline.parse(handle)

# Keep in mind that records is an iterator, so you can iterate through the records only once. If you want to save the records, you can convert them to a list:

records = list(records)

np.savetxt('SystematicSearch.txt', records, fmt="%s") # this saves in a text format

# Let's now iterate over the records to print out some information about each record:

#for record in records:
#	print "title:", record["TI"]
#	if "AU" in records:
#		print "authors:", record["AU"]
#	print "source:", record["CO"]
#	print



# The output for this looks like:
# title: 
# authors: ['Name1 AB', 'Name2 CD', 'Name3 EF', 'Name4 GH']
# source: J Comp Physiol [A] 2000 Jun;186(6):567-74

 














