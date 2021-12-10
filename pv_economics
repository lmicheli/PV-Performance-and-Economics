import numpy as np

def calculate_lcoe(Ef, econ, T=25, deg=0.01, Nd=20):
    r"""
    Calculate the LCOE, in €/kWh, assuming linear degradation rate.

    Parameters
    ----------
    Ef (float) [kWh/kW/year]: Yearly energy yield of the system, degradation excluded.
    econ (numeric) [€/kW]: Contains system specific economic information. Fractional values are reported as percentages. It should include:
        capex (float) [€/kW]: Capital Expenditure
        opex (float) [€/kW/year]: Yearly O&M Costs
        Tx (float) [%]: Corporate Tax Rate
        d (float) [%]: Discount Rate
        rom (float) [%]: Annual increase in O&M Costs
    T (int) [year]: Lifetime of the system
    deg (numeric) [year-1]: Yearly Linear System's Degradation rate
    Nd (int) [year]: Number of years considered for depreciation

    Returns
    -------
    lcoe (numeric) [€/kWh]: LCOE
    
    """    
    #Calculating Annual Tax Depreciation
    Dn=econ.capex/Nd
    #Setting Values of Numerator and Denominator before Year 1
    lcoe_num=econ.capex
    lcoe_den=0.
    #Iteration over the PV system lifetime
    for year in np.arange(1,T+1):
        #Add the yearly OPEX costs to numerator
        lcoe_num+=econ.opex*(1-econ.Tx/100.)*(1+econ.rom/100.)**year/(1+econ.d/100.)**year
        #Substract the depreciation from numerator
        if year<=Nd: lcoe_num-=Dn*econ.Tx/100./(1+econ.d/100.)**year
        #Add the yearly yield, after degradation, to denominator
        lcoe_den+=Ef*(1-deg)**year/(1+econ.d/100.)**year    
    #Return the value of the ratio between numerator and denominator
    return lcoe_num/lcoe_den
