kbt dayo!!!
satoko dayo!!! \n
karitekitaneko

Main(ここ以下をコピペ)
////////
import androidx.appcompat.app.AppCompatActivity;
import android.os.Bundle;
import android.content.Intent;
import android.net.Uri;
import android.view.View;
import android.widget.Button;
import android.widget.ImageView;
import android.widget.Toast;
import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;

public class MainActivity extends AppCompatActivity {
private static final int PICK_FILE_REQUEST = 1;
private ImageView imageView;
private File selectedFile;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        imageView = findViewById(R.id.imageView);
        Button button = findViewById(R.id.button);

        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                // ファイルを選択するインテントを作成し、ギャラリーアプリを開く
                Intent intent = new Intent(Intent.ACTION_GET_CONTENT);
                intent.setType("*/*");
                startActivityForResult(intent, PICK_FILE_REQUEST);
            }
        });
    }

    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);

        if (requestCode == PICK_FILE_REQUEST && resultCode == RESULT_OK) {
            // 選択されたファイルのURIを取得
            Uri fileUri = data.getData();
            if (fileUri != null) {
                // ファイルを保存して表示
                saveAndDisplayFile(fileUri);
            }
        }
    }

    private void saveAndDisplayFile(Uri fileUri) {
        // 選択されたファイルを内部ストレージに保存
        try {
            InputStream inputStream = getContentResolver().openInputStream(fileUri);
            selectedFile = new File(getFilesDir(), "selected_file");
            OutputStream outputStream = new FileOutputStream(selectedFile);

            byte[] buffer = new byte[1024];
            int length;
            while ((length = inputStream.read(buffer)) > 0) {
                outputStream.write(buffer, 0, length);
            }

            outputStream.close();
            inputStream.close();

            // ファイルの保存が成功した場合、ImageViewに表示
            imageView.setImageURI(Uri.fromFile(selectedFile));
        } catch (IOException e) {
            e.printStackTrace();
            Toast.makeText(getApplicationContext(), "Failed to save file", Toast.LENGTH_SHORT).show();
        }
    }
}
//////

xml(ここ以下をコピペ)
//////
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
xmlns:tools="http://schemas.android.com/tools"
android:layout_width="match_parent"
android:layout_height="match_parent"
tools:context=".MainActivity">

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_centerInParent="true"
        android:text="Select File" />

    <ImageView
        android:id="@+id/imageView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_below="@id/button"
        android:layout_centerHorizontal="true"
        android:layout_marginTop="20dp" />

</RelativeLayout>

////

