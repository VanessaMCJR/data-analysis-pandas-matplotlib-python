# data-analysis-pandas-matplotlib-python

Use pandas for the arrays and matplotlib for the graphs

code python : main.py

Example:

suicides_ratio = pandas.DataFrame(ratio(suicides), index =["Ratios origine par suicide"]).transpose() 


accidentals_ratio = pandas.DataFrame(ratio(accidentals), index = ["Ratios origine par accidents"]).transpose()

undetermined_ratio =pandas.DataFrame (ratio(undetermined), index = ["Ratios origine par undetermined"]).transpose()

na_ratio = pandas.DataFrame (ratio(na), index =["Ratios origine par NA"]).transpose()

masculin_ratio = pandas.DataFrame (ratio(m), index =["Ratios par origine hommes"]).transpose()

feminin_ratio = pandas.DataFrame (ratio(f), index =["Ratios par origine femmes"]).transpose()

a2012_ratio = pandas.DataFrame (ratio(a2012), index =["Ratios origine pour 2012"]).transpose()

a2013_ratio = pandas.DataFrame (ratio(a2013), index =["Ratios origine pour 2013"]).transpose()

a2014_ratio = pandas.DataFrame (ratio(a2014), index =["Ratios origine pour 2014"]).transpo


array_final = pandas.concat([homicides_columns, suicides_columns,accidentals_columns, undetermined_columns,na_columns,f_columns,m_columns,a2012_columns,a2013_columns,a2014_columns], axis = 1, sort = True).sort_index(axis = 0, ascending = True)


