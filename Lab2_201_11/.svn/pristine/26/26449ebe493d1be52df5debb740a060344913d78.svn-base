package ca.uwaterloo.Lab2_201_11;

//import java.util.Arrays;

import java.util.Arrays;

//import ca.uwaterloo.Lab2_201_11.LightSensorEventListener;
import ca.uwaterloo.Lab2_201_11.LineGraphView;
//import ca.uwaterloo.Lab2_201_11.MagneticFieldSensorEventListener;
import ca.uwaterloo.Lab2_201_11.R;
//import ca.uwaterloo.Lab2_201_11.RotationVector;
import ca.uwaterloo.Lab2_201_11.accelerometer;

import android.support.v7.app.ActionBarActivity;
import android.support.v7.app.ActionBar;
import android.support.v4.app.Fragment;
import android.graphics.Color;
import android.hardware.Sensor;
import android.hardware.SensorEvent;
import android.hardware.SensorEventListener;
import android.hardware.SensorManager;
import android.os.Bundle;
import android.view.Gravity;
import android.view.LayoutInflater;
import android.view.Menu;
import android.view.MenuItem;
import android.view.View;
import android.view.ViewGroup;
import android.widget.Button;
import android.widget.LinearLayout;
import android.widget.TextView;
import android.os.Build;



import android.support.v7.app.ActionBarActivity;
import android.support.v7.app.ActionBar;
import android.support.v4.app.Fragment;
import android.os.Bundle;
import android.view.LayoutInflater;
import android.view.Menu;
import android.view.MenuItem;
import android.view.View;
import android.view.ViewGroup;
import android.os.Build;

public class MainActivity extends ActionBarActivity {
		
	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);
		if (savedInstanceState == null) {
			getSupportFragmentManager().beginTransaction()
					.add(R.id.container, new PlaceholderFragment()).commit();
		}
	}

	@Override
	public boolean onCreateOptionsMenu(Menu menu) {
		// Inflate the menu; this adds items to the action bar if it is present.
		getMenuInflater().inflate(R.menu.main, menu);
		return true;
	}

	@Override
	public boolean onOptionsItemSelected(MenuItem item) {
		// Handle action bar item clicks here. The action bar will
		// automatically handle clicks on the Home/Up button, so long
		// as you specify a parent activity in AndroidManifest.xml.
		int id = item.getItemId();
		if (id == R.id.action_settings) {
			return true;
		}
		return super.onOptionsItemSelected(item);
	}

	/**
	 * A placeholder fragment containing a simple view.
	 */
	public static class PlaceholderFragment extends Fragment {
		
		public PlaceholderFragment() {
		}

		@Override
		public View onCreateView(LayoutInflater inflater, ViewGroup container,
				Bundle savedInstanceState) {
			View rootView = inflater.inflate(R.layout.fragment_main, container,
					false);
			LinearLayout layout = (LinearLayout) rootView.findViewById(R.id.label2);
	 		
			            //sets layout to vertical
			            
			            layout.setOrientation(LinearLayout.VERTICAL);
			            
			            //LineGraphView object creation
			            
			            LineGraphView graph = new LineGraphView(rootView.getContext(),100,Arrays.asList("x","y","z"));
			            layout.addView(graph);
			            graph.setVisibility(View.VISIBLE);
			            
			            //Redundant code Please remove afterwards
			            //Redundant code from Lab1
			            
			            //TextView tv = (TextView) rootView.findViewById(R.id.label1);
						//tv.setText("I’ve replaced the label!");
						//TextView tv1 = new TextView(rootView.getContext());
						//tv1.setText("Example Text");
						//tv1.findViewById(R.id.label2);
						//layout.addView(tv1);
						
			            //Redundant code Please remove afterwards
			            
			            //Declares Sensor  
						
					    SensorManager sensorManager = (SensorManager) rootView.getContext().getSystemService(SENSOR_SERVICE);
					            
			            //Accelerometer Starts
					
					    //new variable creation for lab2
						
						TextView accelWalk = new TextView(rootView.getContext());
						accelWalk.setText("TEST FOR LAB2");
						layout.addView(accelWalk);
			            
					    //new variable creation for lab2 
						
			            Sensor AccelerometerSensor = sensorManager.getDefaultSensor(Sensor.TYPE_LINEAR_ACCELERATION);
			            SensorEventListener a = new accelerometer(accelWalk, graph);
			            sensorManager.registerListener(a, AccelerometerSensor, SensorManager.SENSOR_DELAY_FASTEST);
			            
			            //Accelerometer ends
	                    
			            //Reset Button for Resetting the number of Steps to zero
			            
			          Button clear = new Button(rootView.getContext()); //Creates an object which is of type button 
			          clear.setTextColor(Color.parseColor("#0000CC")); //text in blue colour
			          clear.setText("Reset Value");
			          layout.addView(clear);
			          
			          clear.setOnClickListener(new View.OnClickListener() {
			              public void onClick(View v) {
			                  
			            	  // Perform action on click
			            	  // Complete reset of values
			            	  // stepsTaken set to zero
			            	  // Syntax obtained from android documentation
			            	
			            	  ca.uwaterloo.Lab2_201_11.accelerometer.stepsTaken = 0;
			              }
			          });
			          
			           return rootView;
			           
		}
	}
}


class accelerometer implements SensorEventListener {
	
	
	// Declaration of two variables,one to count the number of steps
	// and the other to use in the state machine algorithm so it helps
	// us clearly define what a step is
	
    public static int positionVariableForSteps = 0;
	public static int stepsTaken = 0;
		
	TextView output;
	LineGraphView graphtest;
	public accelerometer(TextView accelWalk, LineGraphView graphCopy) {
		
		output = accelWalk;
		graphtest = graphCopy;
	}
	
	public void onAccuracyChanged(Sensor S, int i) {}
	
	public void onSensorChanged(SensorEvent se) {
		
		    //Putting the values of "se.values" array into a float array in order 
		    //to pass it to the addPoint method of LineGraphView.java which takes in a 
		    //float array as an argument
		
		    float[] statemachinevalues = new float[se.values.length];
		    if(se.sensor.getType() == Sensor.TYPE_LINEAR_ACCELERATION) {
			
			//algorithm for low pass filter 
			
			statemachinevalues = se.values;
			
			// Forcibly set the x and z axis values to zero as we're not using it
			// This is done as a precautionary measure so as to maintain accuracy 
			// in our preferred axis of choice(in this case the Y-axis) so as to 
			// count the steps
			
			statemachinevalues[0] = 0;
			statemachinevalues[2] = 0;
			
			
			// Only using the y-axis.
			// The z-axis and the x-axis are not used and hence their values are 
			// immediately and forcibly set to zero
			
			
			// Filter algorithm to filter out the noise as given per the Lab Manual
			
			statemachinevalues[1] += (se.values[1] - statemachinevalues[1]) / 2.5; //arbitrary constant for c
			
			// The value of the Constant "C" is arbitary as long as it is sufficently low enough
			// For optimal results one might use the value of C between 2 and 3
					
			
			// Each step that you take consists of an increasing function
			// So every step you take consists of the function going from a 
			// Decreasing value to an increasing value and back to a decreasing value
			// So it basically means that in order for the step to be counted
			// One would have to calculate the the fall state, the peak state 
			// and the rise state (and possibly the small negative value that 
			// arises when we finish taking one step) and use those values
			// to approximate our state machine algorithm so that it knows	
			// when a step is to be registered. 
			
			// Rising State of the curve
			if (positionVariableForSteps == 0 && statemachinevalues[1] >= 0.1 && statemachinevalues[1] <= 0.2){
				positionVariableForSteps = 1;				
			}
			
			// If the filtered value is indeed in the rising state then 
			// the "positionVariableForSteps" is incremented by 1 so as to 
			// let us know that the person has just begun to take a step
			
			// Peak State of the curve
			if (positionVariableForSteps == 1 && statemachinevalues[1] >= 0.6 && statemachinevalues[1] <= 1.5){
				positionVariableForSteps = 2; 			
			}
			
			// If the filtered value is indeed in the peak state then 
			// the "positionVariableForSteps" is incremented by 1 so as to 
			// let us know that the person is in the middle of taking a step
			
			// Falling state of the curve
			if (positionVariableForSteps == 2 && statemachinevalues[1] >= 0.6 && statemachinevalues[1] <= 1.2){
				positionVariableForSteps = 3;				
			}
			
			// If the filtered value is indeed in the falling state then 
			// the "positionVariableForSteps" is incremented by 1 so as to 
			// let us know that the person has finished taking a step
			
			/*
			 * 
			 * --->  FALSE POSITIVE VALUES TO BE NOTED <----
			//Small Negative State that occurs while walking
			
			if (positionVariableForSteps == 2 && statemachinevalues[1] >= 0 && statemachinevalues[1] <= 0.1){
				positionVariableForSteps = 4;
			}
			
			*FALSE POSITIVE VALUES TO BE NOTED
			*/	
			
			// Now if the "positionVariableForSteps" has a value of three then
			// we know for sure that a step has been completed and now the next 
			// thing to do would be to count the step just taken
			// Hence we increment the "stepsTaken" variable by 1
			
			if (positionVariableForSteps == 3){
				stepsTaken++;
				positionVariableForSteps = 0;   
			}
			
			// We also reset the "positionVariableForSteps" to zero
            // because this serves as an iterative loop and we have to 
			// start checking if the next step has been taken or not
			
			
			//output.setText("\n Steps Counter : " + stepsTaken + "\n" + " Position : " + positionVariableForSteps);
			output.setText("\n Steps Counter : " + stepsTaken + "\n");
			
			// Centre align the output
			// Source : Android Documentation TextView.setGravity
		
			output.setGravity(Gravity.CENTER_HORIZONTAL);
			
			// adds the respective points to the graph using the addPoint method
			// We can actually see from the graph the corresponding state machine 
			// values when we are in the process of taking a step
			// ie: namely rise state, peak state and fall state
			
			graphtest.addPoint(statemachinevalues);		
		
			
		}	
	}
}
