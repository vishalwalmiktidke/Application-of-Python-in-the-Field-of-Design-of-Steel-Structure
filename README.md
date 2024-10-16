# Application-of-Python-in-the-Field-of-Design-of-Steel-Structure
ASSIGNMENT NO. 10


Q.1) # Design of Tension Member

# Input ultimate and yield strengths
Tu = float(input("Enter the value of Ultimate tensile strength (kN/m^2): "))
fy = float(input("Enter the value of yield strength of steel (kN/m^2): "))
Fu = float(input("Enter the value of ultimate strength of steel (kN/m^2): "))
fub = float(input("Enter the value of ultimate strength of bolt (kN/m^2): "))
Gamma_mo = float(input("Enter the value of partial factor of safety Gamma_mo: "))
Gamma_m1 = float(input("Enter the value of partial factor of safety Gamma_m1: "))
Gamma_mb = float(input("Enter the value of partial factor of safety Gamma_mb: "))

# Gross Area Required
Agreq = 1.1 * Tu * 1000 / fy  # Convert kN to N
print("Gross Area Required: ", Agreq)

# Selection of section
Ag = float(input("Enter the value of gross area of steel (mm^2): "))
Lcl = float(input("Enter the length of connected leg (mm): "))
Lol = float(input("Enter the length of outstand leg (mm): "))
t = float(input("Enter the value of least thickness (mm): "))

# Design of Connections
d = float(input("Enter the value of diameter of bolt (mm): "))
do = d + 2
print("The diameter of bolt hole is:", do)

# Minimum pitch distance
pin = 2.5 * d
print("The minimum pitch is:", pin)

# Edge distance
e = 1.56 * do
print("Edge distance:", e)

# Shear plane calculations
nn = float(input("Number of shear planes with threads intercepting the shear plane: "))
ns = float(input("Number of shear planes without threads: "))
Anb = 0.78 * 0.7854 * (d ** 2)
print("Threaded area of bolt is:", Anb)
Asb = 0.7854 * (d ** 2)
print("Plain shank area of bolt is:", Asb)

# Bolt shear strength
Vdsb = (fub / (1.732 * Gamma_mb)) * (nn * Anb + ns * Asb) * 10 ** -3  # Conversion to kN
print("The value of Vdsb:", Vdsb)

# K factors calculations
Kb1 = e / (3 * do)
print("Kb1:", Kb1)
Kb2 = (pin / (3 * do)) - 0.25
print("Kb2:", Kb2)
Kb3 = fub / Fu
print("Kb3:", Kb3)
Kb4 = 1
print("Kb4:", Kb4)

# Minimum K factor
kb = min(Kb1, Kb2, Kb3, Kb4)
print("kb:", kb)

# Bolt design strength
Vdpb = (2.5 * kb * d * t * Fu * 10 ** -3) / Gamma_mb  # Conversion to kN
print("Vdpb:", Vdpb)

# Design strength calculation
Vd = min(Vdsb, Vdpb)
print("Vd:", Vd)

# Number of bolts required
N = Tu / Vd
print("Number of bolts required:", N)

# User-defined number of bolts
N_user = int(input("Enter the value of number of bolts: "))

# Check for strength
# Criteria 1: Yielding of Gross Section
Tdg = (Ag * fy * 10 ** -3) / Gamma_mo  # Conversion to kN
print("Tensile strength due to yielding of gross section:", Tdg)

# Criteria 2: Rupture
Anc = (Lcl - (t / (2 * do))) * t  # Net Area of connecting leg
print("Net Area of connecting leg (Anc):", Anc)
Ago = (Lol - (t / 2)) * t  # Gross Area of outstand leg
print("Gross Area of outstand leg (Ago):", Ago)
Lc = (N_user - 1) * pin  # Effective length
print("le:", Lc)

# Assuming 'bs' is the overall length
bs = 0.6 * Lcl + Lol * t
print("bs:", bs)

# Beta calculation
Beta = (fy / Fu) * (bs / Lc) * (Lol / t) * 1.4  # Placeholder, adjust as needed
print("Beta:", Beta)

# Check 1 for Beta
if Beta > 1.4:
    print("NOT SAFE")
else:
    print("SAFE")

# Check 2 for Tdn
if Beta < 0.7:
    print("NOT SAFE")
else:
    print("SAFE")
    Tdn = ((0.9 * Fu * Anc) / Gamma_m1) + (Beta * Ago * fy / Gamma_mo)
    print("Tdn:", Tdn)

# Criteria 3: Block Shear
Avg = (pin * (N_user + 1) + e) * t
print("Avg:", Avg)
Avn = ((pin * (N_user + 1) + e) - ((N_user + 1) * do + (8.5 * do))) * t
print("Avn:", Avn)
Atg = 0.6 * Lcl * t
print("Atg:", Atg)
Atn = Atg * 0.5 * do
print("Atn:", Atn)

# Block shear calculations
Tb1 = ((Avg * fy) / (1.732 * Gamma_mo)) + ((0.9 * Fu * Atn) / Gamma_m1) * 10 ** -3
print("Tb1:", Tb1)
Tb2 = ((0.9 * Avn * Fu) / (1.732 * Gamma_m1)) + ((Atg * fy) / Gamma_mo) * 10 ** -3
print("Tb2:", Tb2)

# Minimum block shear strength
Tb = min(Tb1, Tb2)
print("Tb:", Tb)

# Overall design strength check
Td = min(Tdg, Tdn, Tb)
print("Td:", Td)

if Td > Tu:
    print("SAFE")
else:
    print("Revise the section")



OUTPUT:
Enter the value of Ultimate tensile strength (kN/m^2): 225
Enter the value of yield strength of steel (kN/m^2): 250
Enter the value of ultimate strength of steel (kN/m^2): 410
Enter the value of ultimate strength of bolt (kN/m^2): 400
Enter the value of partial factor of safety Gamma_mo: 1.1
Enter the value of partial factor of safety Gamma_m1: 1.25
Enter the value of partial factor of safety Gamma_mb: 1.25
Gross Area Required:  990.0000000000001
Enter the value of gross area of steel (mm^2): 1188
Enter the length of connected leg (mm): 100
Enter the length of outstand leg (mm): 65
Enter the value of least thickness (mm): 8
Enter the value of diameter of bolt (mm): 20
The diameter of bolt hole is: 22.0
The minimum pitch is: 50.0
Edge distance: 34.32
Number of shear planes with threads intercepting the shear plane: 1
Number of shear planes without threads: 0
Threaded area of bolt is: 245.0448
Plain shank area of bolt is: 314.15999999999997
The value of Vdsb: 45.273866050808316
Kb1: 0.52
Kb2: 0.5075757575757576
Kb3: 0.975609756097561
Kb4: 1
kb: 0.5075757575757576
Vdpb: 66.59393939393941
Vd: 45.273866050808316
Number of bolts required: 4.969754510195687
Enter the value of number of bolts: 5
Tensile strength due to yielding of gross section: 270.0
Net Area of connecting leg (Anc): 798.5454545454545
Gross Area of outstand leg (Ago): 488.0
le: 200.0
bs: 580.0
Beta: 20.114329268292682
NOT SAFE
SAFE
Tdn: 2466592.591574279
Avg: 2674.56
Avn: 122.55999999999995
Atg: 480.0
Atn: 5280.0
Tb1: 352513.9362855343
Tb2: 20998.0701238715
Tb: 20998.0701238715
Td: 270.0
SAFE
