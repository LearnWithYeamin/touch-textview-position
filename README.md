<p align="center">
<p align="center">
  <a href="https://github.com/i-rin-eam">
    <img src="https://avatars.githubusercontent.com/u/154800878?s=400&u=5d18880cc28646190a19a971bfcdbc54644eab07&v=4" alt="Logo" width="100" height="100">
  </a> 
<h2 align='center'>TextView Manipulation with Drag and Adjustable Text Size using SeekBar in Android</h12>
</p>

## Step 1: Here is `activity_main.xml` code.
```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">


    <!-- Parent Layout with fixed height -->
    <RelativeLayout
        android:id="@+id/parentLayout"
        android:layout_width="match_parent"
        android:layout_height="500dp"
        android:background="#EEEEEE">

        <!-- Draggable TextView -->
        <TextView
            android:id="@+id/draggableTextView"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Drag Me"
            android:textSize="18sp"
            android:background="#FFC107"
            android:padding="10dp"
            android:layout_centerInParent="true" />
    </RelativeLayout>

    <!-- SeekBar for adjusting text size -->
    <SeekBar
        android:id="@+id/textSizeSeekBar"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:max="100"
        android:progress="18"
        android:layout_below="@id/parentLayout"
        android:layout_marginTop="16dp" />

    <!-- Reset Button -->
    <Button
        android:id="@+id/resetButton"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Reset"
        android:layout_below="@id/textSizeSeekBar"
        android:layout_centerHorizontal="true"
        android:layout_marginTop="16dp"/>
</RelativeLayout>
```
## Step 2: Here is `MainActivity.java` code.
```java
   package com.myeamin.newtestpro;

import android.os.Bundle;
import android.view.MotionEvent;
import android.view.View;
import android.widget.Button;
import android.widget.RelativeLayout;
import android.widget.SeekBar;
import android.widget.TextView;

import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    private TextView draggableTextView;
    private Button resetButton;
    private RelativeLayout parentLayout;
    private SeekBar textSizeSeekBar;
    private float dX, dY;
    private int lastAction;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        draggableTextView = findViewById(R.id.draggableTextView);
        resetButton = findViewById(R.id.resetButton);
        parentLayout = findViewById(R.id.parentLayout);
        textSizeSeekBar = findViewById(R.id.textSizeSeekBar);

        // Handle SeekBar changes for text size
        textSizeSeekBar.setOnSeekBarChangeListener(new SeekBar.OnSeekBarChangeListener() {
            @Override
            public void onProgressChanged(SeekBar seekBar, int progress, boolean fromUser) {
                // Set the text size of TextView based on SeekBar progress
                draggableTextView.setTextSize(progress);
            }

            @Override
            public void onStartTrackingTouch(SeekBar seekBar) {
                // You can handle actions when touch starts, if needed
            }

            @Override
            public void onStopTrackingTouch(SeekBar seekBar) {
                // You can handle actions when touch stops, if needed
            }
        });

        draggableTextView.setOnTouchListener(new View.OnTouchListener() {
            @Override
            public boolean onTouch(View view, MotionEvent motionEvent) {

                int actionMasked = motionEvent.getActionMasked();
                if (actionMasked == MotionEvent.ACTION_DOWN) {// Get the position of TextView relative to the screen
                    dX = view.getX() - motionEvent.getRawX();
                    dY = view.getY() - motionEvent.getRawY();
                    lastAction = MotionEvent.ACTION_DOWN;
                } else if (actionMasked == MotionEvent.ACTION_MOVE) {// Calculate new X and Y coordinates for TextView
                    float newX = motionEvent.getRawX() + dX;
                    float newY = motionEvent.getRawY() + dY;

                    // Get the boundaries of the parent layout
                    int parentWidth = parentLayout.getWidth();
                    int parentHeight = parentLayout.getHeight();

                    // Ensure the TextView doesn't move outside the parent layout
                    if (newX >= 0 && newX <= parentWidth - view.getWidth()) {
                        view.setX(newX);
                    }
                    if (newY >= 0 && newY <= parentHeight - view.getHeight()) {
                        view.setY(newY);
                    }
                    lastAction = MotionEvent.ACTION_MOVE;
                } else if (actionMasked == MotionEvent.ACTION_UP) {// If finger was lifted up after dragging
                    if (lastAction == MotionEvent.ACTION_DOWN) {
                        // Add any click action here if necessary
                    }
                } else {
                    return false;
                }
                return true;
            }
        });

        // Reset button to move the TextView back to the original position and text size
        resetButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                // Reset the position of TextView to the center of the parent layout
                draggableTextView.setX((parentLayout.getWidth() - draggableTextView.getWidth()) / 2);
                draggableTextView.setY((parentLayout.getHeight() - draggableTextView.getHeight()) / 2);

                // Reset the text size to default value
                textSizeSeekBar.setProgress(18); // Default text size
                draggableTextView.setTextSize(18);
            }
        });
    }
}

```
## Authors

**MD YEAMIN** - Android Software Developer <a href="https://www.youtube.com/@LearnWithYeamin">**(Learn With Yeamin)**</a> 

<h1 align="center">Thank You ❤️</h1>
