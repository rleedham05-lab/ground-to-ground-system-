# ground-to-ground-system-
this system uses ground to ground calculation in combination with graphs 
import os
import math
import sys
import time
import matplotlib.pyplot

# --- SETUP FUNCTIONS 

# clear screen function
def clear_screen():
    clear = lambda: os.system('clear')
    clear()

# delay print function
def delay_print(s):
    """Prints a string character by character with a small delay."""
    for c in s:
        sys.stdout.write(c)
        sys.stdout.flush()
        time.sleep(0.010)
    sys.stdout.write('\n')

#  VALIDATION FUNCTION 

def value_checker(v_value, angle_value):
    """Checks if both input values can be successfully converted to floats."""
    try:
        # Attempt to convert both inputs to floats within the try block
        float(v_value)
        float(angle_value)
        
        # If we reach this point, both conversions succeeded.
        delay_print("✅ input accepted.")
        return True
        
    except ValueError:
        # If a ValueError was raised, the input was not a valid number.
        delay_print("❌ Invalid input detected. Please input valid numbers for both velocity and angle.")
        return False
        


# time peak of projectile 
def projectile_peak_time(v, angle_degrees, g):
    angle_radians = math.radians(angle_degrees) 
    T_peak = (v * math.sin(angle_radians)) / g
    return T_peak
    
# Finding projectile peak height 
def projectile_peak(v, angle_degrees,g):
    angle_radians = math.radians(angle_degrees)
    #calculating Y_max 
    Y_max = (v * math.sin(angle_radians))**2/(2*g)
    return Y_max

#fiding time projectile spent in motion 
def calculate_time(v, angle_degrees, g):
    T_peak = projectile_peak_time(v, angle_degrees, g)
    Y_max = projectile_peak(v, angle_degrees, g)
    Time = 2*T_peak
    
    return Time 

def range_calculator(total_time,v,angle):
    angle_degrees = angle 
    angle_radians = math.radians(angle_degrees)
    #calculating range using vertical displacment equation 
    Range = (v*math.cos(angle_radians))*total_time
    return Range 

# --- MAIN EXECUTION ---

# intial interface for this function 
finished = False
while finished == False :
    #setting constants and vectors 
    g = 9.81  # Acceleration due to gravity (constant)
    # Initialize main calculation variables
    v = 0
    Range = 0 
    # main variables for vector quantities 
    x_velocity = 0 
    y_velocity = 0
    x_displacment = 0
    y_displacment = 0
    angle = 0
    mass = 0
      
    correct = False 
    while correct == False:
        # Start of outputs screen
        delay_print("PROJECTILE MOTION CALCULATOR")
        delay_print("============================")
        delay_print("You must enter Mass, Velocity, and Angle to continue.")
        delay_print("Type Y to continue")
        
        # User input loop for 'Y'
        correct_2 = False 
        while correct_2 == False:
            choice = input(">> ").upper()
            if choice == "Y":
                correct_2 = True  
                clear_screen()
            else:
                delay_print("Please type 'Y' to continue.")

        # Initial finding of mass 
        delay_print("What is the mass of the desired projectile (kg)?")
        correct_2 = False  
        while correct_2 == False:
            m_str = input("Mass (kg) >> ")
            try:
              #checks user input is numeric 
                m_float = float(m_str)
                if m_float <= 0:
                     delay_print("Mass must be greater than zero.")
                     continue
                mass = m_float 
                correct_2 = True  
                clear_screen()
            except ValueError:
                delay_print("❌ Please input a valid numeric value for mass.")
        
        # Finding value for velocity and the value of the angle 
        correct_2 = False  
        while correct_2 == False:
            delay_print("Please input the values you are aware of:")
            delay_print("1. What is the velocity of the object (m/s)?")
            delay_print("If this value is unknown, please type 'N/A'")
            
            v_input = input("Velocity (m/s) >> ").upper()

            # Subroutine for finding the value of v from kinetic energy
            if v_input == "N/A":
                clear_screen()
                delay_print("Please input the value of the kinetic energy (Joules):")
                
                # Nested loop for safe kinetic energy input
                k = 0
                k_correct = False
                while k_correct == False:
                    k_str = input("Kinetic Energy (J) >> ")
                    try:
                        k = float(k_str)
                        if k < 0:
                            delay_print("Kinetic energy cannot be negative.")
                            continue
                        k_correct = True
                    except ValueError:
                        delay_print("❌ Invalid input for kinetic energy.")
                
                # Calculate v from K.E. (K = 0.5 * m * v^2)
                v = math.sqrt((2 * k) / mass) 
                delay_print("Calculated velocity (v): {v:.2f} m/s")
            else:
                # If user entered a value for v, use it as the string input for the checker
                v = v_input 

            delay_print("2. Please input the angle the projectile was launched at (degrees):")
            angle = input("Angle (degrees) >> ") # Angle is stored as a string

            # Check if v (now either a number or a string) and angle (string) are valid numbers
            correct_2 = value_checker(v, angle)
            
            # If the checker passed, convert the final v and angle variables to floats
            if correct_2:
                # If v was 'N/A', it's already a float. If it was user input, it's a string, so we convert.
                v = float(v) 
                angle = float(angle)
                correct = True # Break out of the outer 'correct' loop too

    # CALCULATE RESULTS
    
    #decelaration is calculted from mass timesed by the gravitationl decelaration caused by gravity 
    total_time = calculate_time(v,angle,g)
    # find projectile time 
    peak_time = projectile_peak_time(v, angle, g)
    # find the projectile peak 
    max_height = projectile_peak(v, angle, g)
    # next step is to find the range
    Range = range_calculator(total_time,v,angle)
    print(Range)
    print(max_height)
    
    #the current speed of object in y axis 
    def new_speed_y(v,angle,current_time,g):
      angle_degrees = angle
      angle_radians = math.radians(angle_degrees)
      current_speed_y = (math.sin(angle_radians)*v)-(current_time*g)
      return current_speed_y
    
    #the current speed of the object in x axis   
    def new_speed_x(v,angle):
      angle_degrees = angle
      angle_radians = math.radians(angle_degrees)
      current_speed_x = math.cos(angle_radians)*v
      return current_speed_x
    
    #the current displacment of a object in its y axis  
    def new_displacment_y(current_speed_y,current_displacment_y,d_t):
      new_displacment = (current_speed_y*d_t) + current_displacment_y
      current_displacment = new_displacment
      return current_displacment 
    
    #the current displacment of a object in its x axis 
    def new_displacement_x(current_speed_x,current_displacment_x,d_t):
      new_displacment = (current_speed_x*d_t) + current_displacment_x
      current_displacment = new_displacment 
      return current_displacment 
    
    # the begining of the use of graphs
    import matplotlib.pyplot as plt
    import numpy as np
    # value for the vector quantities are defined 
    xpoints = np.array([0,Range])
    ypoints = np.array([0, max_height])
    x_value = []
    y_value = []
    x_speed_value =[]
    y_speed_value =[]
    current_time = 0
    current_speed_y = 0
    current_speed_x = 0
    current_displacment_y = 0
    current_displacment_x = 0
    #fonts are definced 
    font1 = {'family':'Sans-serif','color':'black','size':15}
    font2 = {'family':'Sans-serif','color':'black','size':10}  
    #plt.ion are checked to see if could run in the curtrent software 
    try:
        plt.ion()
        print("interactive mode is supported!\n")
        interacative = True 
    except NotImplementedError:
        print("Interactive mode is NOT supported\n")
        interacative = False 
    countinue = input(delay_print("would you like to countinue Y/N")).upper()
    if countinue == "Y":
      done = False
      while done == False:
        delay_print("would you like to have data presented..\n")
        time.sleep(1.0)
        delay_print("1. Real time analysis\n")
        delay_print("2. 100 times speed\n")
        delay_print("3. 1000 times speed\n")
        choice = input(delay_print("please type 1/2/3"))
        if choice == "1":
          d_t = 0.001
          done = True
        elif choice == "2":
          d_t = 0.01
          done = True
        elif choice == "3":
          d_t = 1.0
          done = True
        else:
          delay_print("please input a valid response")
      clear_screen()
      if interacative == True:
      #programe of representation for a software that has the abillity to be interactive 
        while current_displacment_y >= 0:
          #run subroutines to get vectior quantities 
          line, = plt.plot([], [], "b-")
          current_speed_y = new_speed_y(v,angle,current_time,g)
          current_speed_x = new_speed_x(v,angle)
          current_displacment_y = new_displacment_y(current_speed_y,current_displacment_y)
          current_displacment_x = new_displacement_x(current_speed_x,current_displacment_x)
          x_value.append(current_displacment_x)
          y_value.append(current_displacment_y)
          x_speed_value.append(current_speed_x)
          y_speed_value.append(current_speed_y)
          # vector quantities are displayed 
          print("Current speed in X axis (M/S)\n")
          print(current_speed_x)
          print("")
          print("Current speed in Y axis (M/S)\n")
          print(current_speed_y)
          print("")
          print("Current displacment in X axis (M)\n")
          print(current_displacment_x)
          print("")
          print("Current displacment in Y axis (M)\n")
          print(current_displacment_y)
          print("")
          plt.title("Displacment over time ", fontdict = font1)
          plt.xlabel("Height (M)", fontdict = font2)
          plt.ylabel("Range (M)", fontdict = font2)
          line.set_data(x_value, y_value)
          plt.draw()
          plt.pause(0.001)
          current_time = current_time + 0.001
          time.sleep(0.001)
          clear_screen()
      elif interacative == False:
        #subroutine for checking if the progrmae running is not interactive 
        while current_displacment_y >= 0 :
          # vector quantites are defined 
          current_speed_y = new_speed_y(v,angle,current_time,g)
          current_speed_x = new_speed_x(v,angle)
          current_displacment_y = new_displacment_y(current_speed_y,current_displacment_y,d_t)
          current_displacment_x = new_displacement_x(current_speed_x,current_displacment_x,d_t)
          x_value.append(current_displacment_x)
          y_value.append(current_displacment_y)
          x_speed_value.append(current_speed_x)
          y_speed_value.append(current_speed_y)
          print("Current speed in X axis (M/S)\n")
          print(current_speed_x)
          print("")
          print("Current speed in Y axis (M/S)\n")
          print(current_speed_y)
          print("")
          print("Current displacment in X axis (M)\n")
          print(current_displacment_x)
          print("")
          print("Current displacment in Y axis (M)\n")
          print(current_displacment_y)
          print("")
          time.sleep(0.001)
          current_time = current_time + d_t 
          clear_screen()
        #manual plotting 
        plt.title("Displacment over time ", fontdict = font1)
        plt.ylabel("Height (M)", fontdict = font2)
        plt.xlabel("Range (M)", fontdict = font2)
        plt.plot(x_value,y_value)
        plt.show()
        print(x_value)
        print(current_time)
        print(total_time)
        print(peak_time)
    else:
      clear_screen()


    #clear_screen()
    #delay_print("--- PROJECTILE RESULTS ---")
    #delay_print("Launch Velocity (v): {v:.2f} m/s")
    #delay_print("Launch Angle (θ): {angle:.2f}°")
    #delay_print("Projectile Mass: {mass:.2f} kg")
    #delay_print("--------------------------")
    #delay_print("Time to Peak Height: {peak_time:.2f} s")
    #delay_print("Maximum Height (Y_max): {max_height:.2f} m")
    #delay_print("Total Flight Time: {total_time:.2f} s")
    #delay_print("--------------------------")
    
    
