import android.os.Bundle;
import android.os.Handler;
import android.view.MotionEvent;
import android.view.View;
import android.widget.Button;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    private boolean clicking = false;
    private Handler handler = new Handler();
    private final int delay = 100; // Adjust this value to change the click speed

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Button startButton = findViewById(R.id.start_button);
        Button stopButton = findViewById(R.id.stop_button);

        startButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                clicking = true;
                startClicking();
            }
        });

        stopButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                clicking = false;
            }
        });
    }

    private void startClicking() {
        handler.postDelayed(new Runnable() {
            @Override
            public void run() {
                if (clicking) {
                    simulateClick();
                    startClicking(); // Recursive call to keep clicking
                }
            }
        }, delay);
    }

    private void simulateClick() {
        // Simulate touch events
        long downTime = System.currentTimeMillis();
        long eventTime = System.currentTimeMillis() + 100;
        int action = MotionEvent.ACTION_DOWN;
        int x = 500; // Change these values to change the click position
        int y = 500;

        MotionEvent.PointerCoords pointerCoords = new MotionEvent.PointerCoords();
        pointerCoords.x = x;
        pointerCoords.y = y;
        MotionEvent.PointerCoords[] pointerCoordsArray = {pointerCoords};

        MotionEvent event = MotionEvent.obtain(
                downTime,
                eventTime,
                action,
                1,
                new int[]{},
                pointerCoordsArray,
                0,
                0,
                1,
                1,
                0,
                0,
                InputDevice.SOURCE_TOUCHSCREEN,
                0
        );

        dispatchTouchEvent(event);
    }
}
