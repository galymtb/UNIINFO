MainActivity(Главный экран, тот кто приветствует вас)
package com.example.uniinfo;

import androidx.appcompat.app.AppCompatActivity;

import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity implements View.OnClickListener {
    private Button btn;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        getSupportActionBar().hide();
        btn = findViewById(R.id.btn1);//находим кнопку «Изучать» по ID
        btn.setOnClickListener(this);//передаем функцию этой кнопке(Переход в другую страницу)
    }

    @Override
    public void onClick(View v) {
        Toast.makeText(this,"Включите интернет", Toast.LENGTH_SHORT).show();
//Делаем напоминание пользователю о включении интернета
        Intent intent = new Intent(".Main2Activity");
//Создаем переход
        startActivity(intent);
//Реализуем переход
    }

}
Activity_main.xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#2C1C4E"
    tools:context=".MainActivity">

    <ImageView
        android:id="@+id/iv2"
        android:layout_width="match_parent"
        android:layout_height="455dp"
        app:layout_constraintBottom_toTopOf="@+id/tv1"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.0"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.0"
        app:srcCompat="@drawable/icget" />
//Разметка для картинки
    <TextView
        android:id="@+id/tv1"
        android:layout_width="127dp"
        android:layout_height="41dp"
        android:layout_marginBottom="84dp"
        android:text="UNIINFO"
        android:textColor="#FFF"
        android:textSize="30sp"
        android:textStyle="bold"
        app:layout_constraintBottom_toTopOf="@+id/btn1"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent" />
//Надпись «UNIINFO»
    <Button
        android:id="@+id/btn1"
        android:layout_width="300dp"
        android:layout_height="60dp"
        android:layout_marginBottom="52dp"
        android:background="@drawable/bg"
        android:text="Изучать"
        android:textAllCaps="false"
        android:textColor="#FFF"
        android:textSize="30sp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.495"
        app:layout_constraintStart_toStartOf="parent" />
//Кнопка «Изучать»
    <TextView 
        android:id="@+id/textView2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="28dp"
        android:text="Добро Пожаловать"
        android:textColor="#FFFFFF"
        android:textSize="24sp"
        app:layout_constraintBottom_toTopOf="@+id/btn1"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent" />
//Надпись «Добро пожаловать»
</androidx.constraintlayout.widget.ConstraintLayout>
Main2Activity
package com.example.uniinfo;

import android.os.Bundle;
import android.view.MenuItem;
import android.view.View;
import android.widget.Adapter;
import android.widget.AdapterView;
import android.widget.ArrayAdapter;
import android.widget.FrameLayout;
import android.widget.ImageView;
import android.widget.ListView;
import android.widget.TextView;
import android.widget.Toast;

import com.google.android.material.bottomnavigation.BottomNavigationView;

import androidx.annotation.NonNull;
import androidx.appcompat.app.AppCompatActivity;
import androidx.fragment.app.Fragment;
import androidx.fragment.app.FragmentTransaction;

import java.util.ArrayList;
import java.util.List;

public class Main2Activity extends AppCompatActivity {

    private BottomNavigationView navigationView;//Создаем Нижнее меню
    private FrameLayout frameLayout;//Создаем разметку для расположение других элементов над меню
    private usa usa;//призываем фрагмент страницы универов США
    private kz kz; //призываем фрагмент страницы универов КЗ
    private world world; //призываем фрагмент страницы универов мира

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main2);
        getSupportActionBar().hide();

        frameLayout=findViewById(R.id.mainframe);//Находим по ID
        navigationView=findViewById(R.id.nav_view);//Находим по ID

        usa = new usa();
        kz=new kz();
        world=new world();

        setFragment(usa);

        navigationView.setOnNavigationItemSelectedListener(new BottomNavigationView.OnNavigationItemSelectedListener() {//вызываем метод исполнения при нажатии на кнопок в меню
            @Override
            public boolean onNavigationItemSelected(@NonNull MenuItem item) {
                switch (item.getItemId()) {
                    case R.id.navigation_usa://событие при нажатии на кнопку США
                        setFragment(usa);//вызываем фрагмент США
                        return true;
                    case R.id.navigation_kz: //событие при нажатии на кнопку КЗ
                        setFragment(kz);//вызываем фрагмент КЗ
                        return true;
                    case R.id.navigation_world://событие при нажатии на кнопку Мир

                        setFragment(world);//вызываем фрагмент Мир
                        return true;
                }
                return false;

            }
        });
    }

    private void setFragment(Fragment fragment) {
        FragmentTransaction fragmentTransaction =getSupportFragmentManager().beginTransaction();
        fragmentTransaction.replace(R.id.mainframe,fragment);
        fragmentTransaction.commit();

    }
}
USA фрагмент
package com.example.uniinfo;


import android.content.Intent;
import android.graphics.Bitmap;
import android.os.Bundle;

import androidx.fragment.app.Fragment;

import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.AdapterView;
import android.widget.ImageView;
import android.widget.LinearLayout;
import android.widget.ListAdapter;
import android.widget.ListView;
import android.widget.SimpleAdapter;
import android.widget.TextView;
import android.widget.Toast;

import java.util.ArrayList;
import java.util.HashMap;

/**
 * A simple {@link Fragment} subclass.
 */
public class usa extends Fragment {


    public usa() {
        // Required empty public constructor
    }
    String text;//создаем текст передачи при переходе на Page.java
    Intent intent;//создаем переход
    ListView listView;//создаем список
    ImageView imageView;
    TextView textView;
    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        View view =inflater.inflate(R.layout.fragment_usa, container, false);
//создаем вид разметки фрагмента
        DBHelper db = new DBHelper(getActivity(),DBHelper.DATABASE_NAME,DBHelper.DATABASE_VERSION);
//вызываем доступ к базе данных для отображение названии универов в списке
        ArrayList<HashMap<String, String>> univerList = db.GetUnivers("USA");
//Вызов метода предоставления данных по стране
        listView = view.findViewById(R.id.listview);
        imageView=view.findViewById(R.id.imageView);
        textView=view.findViewById(R.id.textView);
        ListAdapter adapter = new SimpleAdapter(getActivity(),univerList,R.layout.listview_item_row,new String[]{"name","namerus"}, new int[]{R.id.textTitle, R.id.textRus});//создаем адаптер для правильной разметки данных в ячейки текстов

        listView.setAdapter(adapter);//стартуем метод адаптера
        imageView.setImageResource(R.drawable.usa);//размещаем картинку флага США в ячейке для фото
        textView.setText("США"););//размещаем текст США в ячейке для текста
        listView.setOnItemClickListener(new AdapterView.OnItemClickListener() {//метод выполнения при нажатия на список
            public void onItemClick(AdapterView<?> parent, View view, int position, long id) {//вызов метода при нажатии на элемент списка в нашем случае на университет
                switch (position) {
                    default://кейс дефолта значит что функция снизу будет работать для всех элементов списка
                        LinearLayout ll = (LinearLayout) view; // извлекаем линейную разметку
                        TextView tv = (TextView) ll.findViewById(R.id.textTitle); // извлекаем текст, это названия универа
                        text = tv.getText().toString();//сохраняем в локальной переменной
                        Toast.makeText(getActivity(), text, Toast.LENGTH_SHORT).show();//озвучивать названия нажатого универа
                        intent = new Intent(getActivity(), Page.class);//переход в класс данных об универе
                        intent.putExtra("txt",text);//отправка названия через переход
                        break;
                }
                startActivity(intent);//старт
            }
        });

        return view;
    }


}
KZ фрагмент
package com.example.uniinfo;


import android.content.Intent;
import android.graphics.Bitmap;
import android.os.Bundle;

import androidx.fragment.app.Fragment;

import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.AdapterView;
import android.widget.ImageView;
import android.widget.LinearLayout;
import android.widget.ListAdapter;
import android.widget.ListView;
import android.widget.SimpleAdapter;
import android.widget.TextView;
import android.widget.Toast;

import java.util.ArrayList;
import java.util.HashMap;

/**
 * A simple {@link Fragment} subclass.
 */
public class kz extends Fragment {


    public kz() {
        // Required empty public constructor
    }
    String text;//создаем текст передачи при переходе на Page.java
    Intent intent;//создаем переход
    ListView listView;//создаем список
    ImageView imageView;
    TextView textView;

    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        View view =inflater.inflate(R.layout.fragment_usa, container, false);
//создаем вид разметки фрагмента

        DBHelper db = new DBHelper(getActivity(),DBHelper.DATABASE_NAME,DBHelper.DATABASE_VERSION);

//вызываем доступ к базе данных для отображение названии универов в списке
        ArrayList<HashMap<String, String>> univerList = db.GetUnivers("KZ");
//Вызов метода предоставления данных по стране
        listView = view.findViewById(R.id.listview);
        imageView=view.findViewById(R.id.imageView);
        textView=view.findViewById(R.id.textView);
        ListAdapter adapter = new SimpleAdapter(getActivity(),univerList,R.layout.listview_item_row,new String[]{"name","namerus"}, new int[]{R.id.textTitle, R.id.textRus});
//создаем адаптер для правильной разметки данных в ячейки текстов
        listView.setAdapter(adapter); //стартуем метод адаптера
        imageView.setImageResource(R.drawable.kz); //размещаем картинку флага КЗ в ячейке для фото
        textView.setText("Казахстан");//размещаем текст КЗ в ячейке для текста


        listView.setOnItemClickListener(new AdapterView.OnItemClickListener() {//метод выполнения при нажатия на список
            public void onItemClick(AdapterView<?> parent, View view, int position, long id) {//вызов метода при нажатии на элемент списка в нашем случае на университет
                switch (position) {
                    default://кейс дефолта значит что функция снизу будет работать для всех элементов списка
                        LinearLayout ll = (LinearLayout) view; // извлекаем линейную разметку                        
                        TextView tv = (TextView) ll.findViewById(R.id.textTitle); // извлекаем текст, это названия универа
                        text = tv.getText().toString();//сохраняем в локальной переменной
                        Toast.makeText(getActivity(), text, Toast.LENGTH_SHORT).show();//озвучивать названия нажатого универа
                        intent = new Intent(getActivity(), Page.class);//переход в класс данных об универе
                        intent.putExtra("txt",text);//отправка названия через переход
                        break;
                }
                startActivity(intent);//старт
            }
        });

        return view;
    }

}
World фрагмент
package com.example.uniinfo;


import android.content.Intent;
import android.graphics.Bitmap;
import android.os.Bundle;

import androidx.fragment.app.Fragment;

import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.AdapterView;
import android.widget.ImageView;
import android.widget.LinearLayout;
import android.widget.ListAdapter;
import android.widget.ListView;
import android.widget.SimpleAdapter;
import android.widget.TextView;
import android.widget.Toast;

import java.util.ArrayList;
import java.util.HashMap;

/**
 * A simple {@link Fragment} subclass.
 */
public class world extends Fragment {


    public world() {
        // Required empty public constructor
    } 
String text;//создаем текст передачи при переходе на Page.java
    Intent intent;//создаем переход
    ListView listView;//создаем список
    ImageView imageView;
    TextView textView;
    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        View view =inflater.inflate(R.layout.fragment_usa, container, false);
//создаем вид разметки фрагмента

        DBHelper db = new DBHelper(getActivity(),DBHelper.DATABASE_NAME,DBHelper.DATABASE_VERSION);
//вызываем доступ к базе данных для отображение названии универов в списке
        ArrayList<HashMap<String, String>> univerList = db.GetUnivers("World");
//Вызов метода предоставления данных по стране
        listView = view.findViewById(R.id.listview);
        imageView=view.findViewById(R.id.imageView);
        textView=view.findViewById(R.id.textView);
        ListAdapter adapter = new SimpleAdapter(getActivity(),univerList,R.layout.listview_item_row,new String[]{"name","namerus"}, new int[]{R.id.textTitle, R.id.textRus});
//создаем адаптер для правильной разметки данных в ячейки текстов
        listView.setAdapter(adapter); //стартуем метод адаптера
        imageView.setImageResource(R.drawable.world); //размещаем картинку флага мира в ячейке для фото
        textView.setText("Мир");
//размещаем картинку флага мира в ячейке для фото

        listView.setOnItemClickListener(new AdapterView.OnItemClickListener() {//метод выполнения при нажатия на список
            public void onItemClick(AdapterView<?> parent, View view, int position, long id) {//вызов метода при нажатии на элемент списка в нашем случае на университет
                switch (position) {
                    default://кейс дефолта значит что функция снизу будет работать для всех элементов списка
                        LinearLayout ll = (LinearLayout) view; // извлекаем линейную разметку                        
                        TextView tv = (TextView) ll.findViewById(R.id.textTitle); // извлекаем текст, это названия универа
                        text = tv.getText().toString();//сохраняем в локальной переменной
                        Toast.makeText(getActivity(), text, Toast.LENGTH_SHORT).show();//озвучивать названия нажатого универа
                        intent = new Intent(getActivity(), Page.class);//переход в класс данных об универе
                        intent.putExtra("txt",text);//отправка названия через переход
                        break;
                }
                startActivity(intent);//старт
            }
        });

        return view;
    }

}

Univer
package com.example.uniinfo;

public class univer {//создаем метод элемента списка то есть универ
    public int icon;
    public String title;//название
    public String rus;//название на русском


    public univer(){
        super();
    }
    public univer(int icon, String title,String rus) {
        super();
        this.icon = icon;
        this.title = title;
        this.rus=rus;
    }

}
univerAdapter
package com.example.uniinfo;

import android.app.Activity;
import android.content.Context;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.ArrayAdapter;
import android.widget.ImageView;
import android.widget.TextView;

public class uniAdapter extends ArrayAdapter<univer> {
    Context context;
    int layoutResourceId;
    univer data[] = null;//создаем массив универов

    public uniAdapter(Context context, int layoutResourceId, univer[] data) {
        super(context, layoutResourceId, data);
        this.layoutResourceId = layoutResourceId;
        this.context = context;
        this.data = data;
    }

    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        View row = convertView;//создаем присваиватель элементов массива к листу
        univerHolder holder = null;

        if(row == null)
        {
            LayoutInflater inflater = ((Activity)context).getLayoutInflater();
            row = inflater.inflate(layoutResourceId, parent, false);

            holder = new univerHolder();
            holder.imgIcon = (ImageView)row.findViewById(R.id.imgIcon);
            holder.textTitle = (TextView)row.findViewById(R.id.textTitle);
            holder.textRus = (TextView)row.findViewById(R.id.textRus);

            row.setTag(holder);
        }
        else
        {
            holder = (univerHolder)row.getTag();
        }

        univer univer = data[position];
        holder.textTitle.setText(univer.title);
        holder.textRus.setText(univer.rus);
        holder.imgIcon.setImageResource(univer.icon);

        return row;
    }

    static class univerHolder
    {
        ImageView imgIcon;
        TextView textTitle;
        TextView textRus;
    }
}
Page
package com.example.uniinfo;

import androidx.appcompat.app.AppCompatActivity;

import android.content.Intent;
import android.database.Cursor;
import android.os.Bundle;
import android.widget.ImageView;
import android.widget.TextView;
import android.widget.Toast;

import com.squareup.picasso.Picasso;

public class Page extends AppCompatActivity {

    DBHelper db;//вызываем БД
    ImageView img;//создаем переменную картинки
    TextView name;//имя универа
    TextView desc;//описания
    TextView country;//страна
    TextView price;//стоимость
    TextView facts;//факты об универах
    TextView faculties;//факультеты
    TextView reqs;//требования к поступлению
    TextView grants;//наличия грантов
    TextView contacts;//контакты
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_page);
        getSupportActionBar().hide();

        img= findViewById(R.id.imgIcon);//Присваиваем ID
        name=findViewById(R.id.textTitle);
        desc=findViewById(R.id.textTitle1);
        country=findViewById(R.id.textTitle2);
        price=findViewById(R.id.textTitle3);
        facts=findViewById(R.id.textTitle5);
        faculties=findViewById(R.id.textTitle7);
        reqs=findViewById(R.id.textTitle9);
        grants=findViewById(R.id.textTitle11);
        contacts=findViewById(R.id.textTitle13);

        Intent intent =getIntent();
        String txt = intent.getStringExtra("txt");//возвращаем текст при переходе
        db= new DBHelper(this,DBHelper.DATABASE_NAME,DBHelper.DATABASE_VERSION);//вызов метода БД
        Cursor cursor =db.alldata(txt);//вызов метода курсор
        cursor.moveToFirst();

        if(cursor.getCount()==0){
            Toast.makeText(getApplicationContext(),"Тут ничего нету пока",Toast.LENGTH_SHORT).show();
        }

        String url=cursor.getString(1);//Присваиваем данные из БД в ячейки
        name.setText(cursor.getString(3));
        desc.setText(cursor.getString(4));
        country.setText(cursor.getString(5));
        price.setText(cursor.getString(6));
        facts.setText(cursor.getString(7));
        faculties.setText(cursor.getString(8));
        reqs.setText(cursor.getString(9));
        grants.setText(cursor.getString(10));
        contacts.setText(cursor.getString(11));

        Picasso.with(this).load(url)//Загрузка картинки с URL ссылки
               .placeholder(R.drawable.loadimg)
                .into(img);
        cursor.close();
    }

}
activity_main2.xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/container"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#2C1C4E"
    android:paddingTop="0dp"
    tools:context=".Main2Activity">



    <com.google.android.material.bottomnavigation.BottomNavigationView
        android:id="@+id/nav_view"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_gravity="bottom"
        android:background="?android:attr/windowBackground"
        app:backgroundTint="#FFFFFF"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:menu="@menu/bottom_nav_menu">

    </com.google.android.material.bottomnavigation.BottomNavigationView>

    <FrameLayout
        android:id="@+id/mainframe"
        android:layout_width="match_parent"
        android:layout_height="673dp"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent">

    </FrameLayout>
Activity_page.xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".Page">

    <ScrollView
        android:id="@+id/sv"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="1dp"
        android:layout_marginBottom="1dp"
        android:background="#26295A"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent">

        <LinearLayout
            android:id="@+id/linear"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:background="#26295A"
            android:gravity="center_vertical"
            android:orientation="vertical">

            <ImageView
                android:id="@+id/imgIcon"
                android:layout_width="fill_parent"
                android:layout_height="395dp"
                android:layout_alignParentTop="true"
                android:layout_alignParentBottom="true"
                android:layout_margin="10dp"
                android:layout_marginLeft="10dp"
                android:layout_marginTop="5dp"
                android:layout_marginRight="10dp"
                android:layout_marginBottom="5dp"
                android:gravity="center_vertical" />

            <TextView
                android:id="@+id/textTitle"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginLeft="10dp"
                android:layout_marginRight="10dp"
                android:text="Массачусетский Технологический Институт"
                android:textColor="#FFFFFF"
                android:textSize="24sp"
                android:textStyle="bold" />

            <TextView
                android:id="@+id/textTitle1"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_marginLeft="10dp"
                android:layout_marginRight="10dp"
                android:text="MIT является элитным частным университетом, расположенным в Кеймбридже, штат Массачусетс. Приемная комиссия данного университета крайне требовательна, что подтверждается показателем приема равным ~3-4% относительно международных абитуриентов. Популярные специальности включают компьютерные науки, машиностроение и математику."
                android:textColor="#FFFFFF" />

            <TextView
                android:id="@+id/textTitle2"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginLeft="10dp"
                android:layout_marginRight="10dp"
                android:gravity="center"
                android:text="USA"
                android:textColor="#FFFFFF"
                android:textSize="24sp"
                android:textStyle="bold" />

            <TextView
                android:id="@+id/textTitle3"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginLeft="10dp"
                android:layout_marginRight="10dp"
                android:gravity="center"
                android:text="Все расходы/Только обучение: 73 160$/53 450$"
                android:textColor="#FFFFFF"
                android:textSize="24sp" />

            <TextView
                android:id="@+id/textTitle4"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginLeft="10dp"
                android:layout_marginRight="10dp"
                android:gravity="center"
                android:text="Facts"
                android:textColor="#FFFFFF"
                android:textSize="24sp"
                android:textStyle="bold" />

            <TextView
                android:id="@+id/textTitle5"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_marginLeft="10dp"
                android:layout_marginRight="10dp"
                android:text="1 января-Дедлайн подачи документов для поступающих;1 место-Среди лучших университетов США, Лучшая цена на обучение в США, Уровень обучения в США"
                android:textColor="#FFFFFF" />

            <TextView
                android:id="@+id/textTitle6"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginLeft="10dp"
                android:layout_marginRight="10dp"
                android:gravity="center"
                android:text="Faculties"
                android:textColor="#FFFFFF"
                android:textSize="24sp"
                android:textStyle="bold" />

            <TextView
                android:id="@+id/textTitle7"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_marginLeft="10dp"
                android:layout_marginRight="10dp"
                android:text="Humanities Cognitive Science Communications Creative Writing Economics English Liberal Arts and Humanities Linguistics, Interpretation, and Translation Music History and Literature Philosophy Political Science and Government Urban, Community and Regional Planning"
                android:textColor="#FFFFFF" />

            <TextView
                android:id="@+id/textTitle8"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginLeft="10dp"
                android:layout_marginRight="10dp"
                android:gravity="center"
                android:text="Requirements"
                android:textColor="#FFFFFF"
                android:textSize="24sp"
                android:textStyle="bold" />

            <TextView
                android:id="@+id/textTitle9"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_marginLeft="10dp"
                android:text="MIT требует результаты SAT/ACT, 2-х SAT Subjects, но не требует SAT optional essay или ACT writing section. Также MIT требует результаты TOEFL, но не принимает результаты IELTS. Минимальных проходных баллов нет, но средний балл поступивших был между 1490-1570 по SAT и 33-35 по ACT. MIT рекомендует набирать как минимум 90 баллов по TOEFL."
                android:textColor="#FFFFFF" />

            <TextView
                android:id="@+id/textTitle10"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginLeft="10dp"
                android:layout_marginRight="10dp"
                android:gravity="center"
                android:text="Grants"
                android:textColor="#FFFFFF"
                android:textSize="24sp"
                android:textStyle="bold" />

            <TextView
                android:id="@+id/textTitle11"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_marginLeft="10dp"
                android:layout_marginRight="10dp"
                android:text="Грант на первый год обучения MIT предоставляет грант для малообеспеченных студентов первого курса в размере 2000$ каждый семестр, чтобы помочь с дополнительными расходами."
                android:textColor="#FFFFFF" />

            <TextView
                android:id="@+id/textTitle12"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginLeft="10dp"
                android:layout_marginRight="10dp"
                android:gravity="center"
                android:text="Contacts"
                android:textColor="#FFFFFF"
                android:textSize="24sp"
                android:textStyle="bold" />
            
            <TextView
                android:id="@+id/textTitle13"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginLeft="10dp"
                android:layout_marginRight="10dp"
                android:layout_marginBottom="10dp"
                android:gravity="center"
                android:text="+1 617-253-1000 admissions@mit.edu 77 Massachusetts Avenue Cambridge, MA 02139 http://web.mit.edu/"
                android:textColor="#FFFFFF"
                android:textSize="18sp" />
        </LinearLayout>
    </ScrollView>
</androidx.constraintlayout.widget.ConstraintLayout>

DBHelper	
package com.example.uniinfo;

import android.content.Context;
import android.database.Cursor;
import android.database.sqlite.SQLiteDatabase;
import android.database.sqlite.SQLiteOpenHelper;

import androidx.annotation.Nullable;

import java.util.ArrayList;
import java.util.HashMap;

public class DBHelper extends SQLiteOpenHelper {

    public static final int DATABASE_VERSION = 1;
    public static final String DATABASE_NAME = "univ.db";

    public DBHelper(@Nullable Context context, @Nullable String name, int version) {
        super(context, DATABASE_NAME, null, DATABASE_VERSION);

    }

    @Override
    public void onCreate(SQLiteDatabase db) {
        String sql = "CREATE TABLE " + MyDataProvider.TABLE_UNIVERSITIES + "("//создаем запрос на добавление таблицы
                + MyDataProvider.KEY_UNIVER_ID + " INTEGER PRIMARY KEY ,"
                + MyDataProvider.KEY_UNIVER_IMG + " TEXT,"
                + MyDataProvider.KEY_UNIVER_NAME + " TEXT,"
                + MyDataProvider.KEY_UNIVER_NAMERUS + " TEXT,"
                + MyDataProvider.KEY_UNIVER_DESCRIPTION + " TEXT,"
                + MyDataProvider.KEY_UNIVER_COUNTRY + " TEXT,"
                + MyDataProvider.KEY_UNIVER_PRICE + " TEXT,"
                + MyDataProvider.KEY_UNIVER_FACTS + " TEXT,"
                + MyDataProvider.KEY_UNIVER_FACULTIES + " TEXT,"
                + MyDataProvider.KEY_UNIVER_REQUIREMENTS + " TEXT,"
                + MyDataProvider.KEY_UNIVER_GRANTS + " TEXT,"
                + MyDataProvider.KEY_UNIVER_CONTACTS + " TEXT" + ")";
        db.execSQL(sql);//запуск запроса
        sql = "INSERT INTO " + MyDataProvider.TABLE_UNIVERSITIES + "("
//создаем запрос на добавление данных в таблицу
                + MyDataProvider.KEY_UNIVER_IMG + ","
                + MyDataProvider.KEY_UNIVER_NAME + ","
                + MyDataProvider.KEY_UNIVER_NAMERUS + ","
                + MyDataProvider.KEY_UNIVER_DESCRIPTION + ","
                + MyDataProvider.KEY_UNIVER_COUNTRY + ","
                + MyDataProvider.KEY_UNIVER_PRICE + ","
                + MyDataProvider.KEY_UNIVER_FACTS + ","
                + MyDataProvider.KEY_UNIVER_FACULTIES + ","
                + MyDataProvider.KEY_UNIVER_REQUIREMENTS + ","
                + MyDataProvider.KEY_UNIVER_GRANTS + ","
                + MyDataProvider.KEY_UNIVER_CONTACTS
                + ") VALUES('https://www.imperial.ac.uk/ImageCropToolT4/imageTool/uploaded-images/MIT-at-Night--tojpeg_1417101385586_x2.jpg'," +
                "'MIT'," +
                "'Массачусетский Технологический Институт'," +
                "'MIT является элитным частным университетом, расположенным в Кеймбридже, штат Массачусетс. Приемная комиссия данного университета крайне требовательна, что подтверждается показателем приема равным ~3-4% относительно международных абитуриентов. Популярные специальности включают компьютерные науки, машиностроение и математику.'," +
                "'USA'," +
                "'Все расходы/Только обучение: 73 160$/53 450$'," +
                "'1 января-Дедлайн подачи документов для поступающих; 1 место-Среди лучших университетов США,Лучшая цена на обучение в США, Уровень обучения в США; 104 700$-В среднем зарабатывают выпускники через 6 лет; 84%-Студентов университета получают финансовую помощь'," +
                "'Arts: Drama and Theatre Production; Business: Business, Management Sciences and Information Systems; Humanities: Cognitive Science, Communications, Creative Writing, Economics, English, Liberal Arts and Humanities, Linguistics, Interpretation, and Translation, Music History and Literature, Philosophy, Political Science and Government, Urban, Community and Regional Planning'," +
                "'MIT требует результаты SAT/ACT, 2-х SAT Subjects, но не требует SAT optional essay или ACT writing section. Также MIT требует результаты TOEFL, но не принимает результаты IELTS. Минимальных проходных баллов нет, но средний балл поступивших был между 1490-1570 по SAT и 33-35 по ACT. MIT рекомендует набирать как минимум 90 баллов по TOEFL.'," +
                "'Стипендия MIT Все студенты которые подают на финансовую помощь рассматриваются на получение данной стипендии'," +
                "'+1 617-253-1000; admissions@mit.edu; 77 Massachusetts Avenue; Cambridge, MA 02139; http://web.mit.edu/')";
        db.execSQL(sql);
        sql="INSERT INTO "+MyDataProvider.TABLE_UNIVERSITIES+"("
                +MyDataProvider.KEY_UNIVER_IMG+","
                +MyDataProvider.KEY_UNIVER_NAME+","
                +MyDataProvider.KEY_UNIVER_NAMERUS+","
                +MyDataProvider.KEY_UNIVER_DESCRIPTION+","
                +MyDataProvider.KEY_UNIVER_COUNTRY+","
                +MyDataProvider.KEY_UNIVER_PRICE+","
                +MyDataProvider.KEY_UNIVER_FACTS+","
                +MyDataProvider.KEY_UNIVER_FACULTIES+","
                +MyDataProvider.KEY_UNIVER_REQUIREMENTS+","
                +MyDataProvider.KEY_UNIVER_GRANTS+","
                +MyDataProvider.KEY_UNIVER_CONTACTS
                +") VALUES('https://www.time-namaz.ru/tmp/1487174403.jpg'," +
                "'Harvard'," +
                "'Гарвардский Университет'," +
                "'Гарвард является элитным частным университетом, расположенным в Кеймбридже, штат Массачусетс. Приемная комиссия данного университета крайне требовательна, что подтверждается показателем приема равным ~2-3% относительно международных абитуриентов. Популярные специальности включают компьютерые науки, политологию и экономику.'," +
                "'USA'," +
                "'Все расходы/Только обучение: 67 580$/46 340$'," +
                "'1 января-Дедлайн подачи документов для поступающих; 1 место-Среди лучших университетов по преподаванию медицины, химии, биологии, математики, политологии, экономики в США; 89 700$-В среднем зарабатывают выпускники через 6 лет; 72%-Студентов университета получают финансовую помощь'," +
                "'Arts: Film and Video Studies, Studio Arts, Visual and Performing Arts; Humanities: African American Studies, Anthropology, Arabic Language and Literature, Archaeology, Art History, Asian Studies, Behavioral Sciences, Chinese Language and Literature, East Asian Languages and Literatures; Science, Technology, and Math: Astronomy and Astrophysics, Bioengineering and Biomedical Engineering, Cellular Biology, Computational and Applied Mathematics'," +
                "'Мотивационное письмо Common App (250-650 слов), 1 мини-эссе на один из поставленных вопросов (100-250 слов), 5 секций для того, чтобы расписать вашу прошлую деятельность, занятия, достижения и т.д. (50-150 слов).Гарвард требует результаты SAT/ACT, рекомендует подать результаты 2-х SAT Subjects, но не требует SAT optional essay или ACT writing section. Также Гарвард требует результаты TOEFL/IELTS. Минимальных проходных баллов нет, но средний балл поступивших был между 1460-1590 по SAT и 32-35 по ACT. Минимальный проходной балл по IELTS равен 7.0.'," +
                "'Студентам с семейным заработком менее 65 000$: Международным студентам выдаются 100%-е гранты на обучение, проживание и питание на полный курс обучения в университете.'," +
                "'+1 617-495-1551; college@fas.harvard.edu; 86 Brattle St; Cambridge, MA 02138; https://www.harvard.edu/')";
        db.execSQL(sql);
        sql="INSERT INTO "+MyDataProvider.TABLE_UNIVERSITIES+"("
                +MyDataProvider.KEY_UNIVER_IMG+","
                +MyDataProvider.KEY_UNIVER_NAME+","
                +MyDataProvider.KEY_UNIVER_NAMERUS+","
                +MyDataProvider.KEY_UNIVER_DESCRIPTION+","
                +MyDataProvider.KEY_UNIVER_COUNTRY+","
                +MyDataProvider.KEY_UNIVER_PRICE+","
                +MyDataProvider.KEY_UNIVER_FACTS+","
                +MyDataProvider.KEY_UNIVER_FACULTIES+","
                +MyDataProvider.KEY_UNIVER_REQUIREMENTS+","
                +MyDataProvider.KEY_UNIVER_GRANTS+","
                +MyDataProvider.KEY_UNIVER_CONTACTS
                +") VALUES('https://uznay-kak.ru/resize/200/200/no/uploads/news/d1ff54b00dda80201588dc3af80e9574.jpg'," +
                "'Stanford'," +
                "'Стэнфордский Университет'," +
                "'Стэнфорд является элитным частным университетом, расположенным в Станфорде, штат Калифорния. Приемная комиссия данного университета крайне требовательна, что подтверждается показателем приема равным ~2-3% относительно международных абитуриентов. Популярные специальности включают компьютерные науки, машиностроение и биологию'," +
                "'USA'," +
                "'Все расходы/Только обучение: 74 570$/53 529$'," +
                "'3 января-Дедлайн подачи документов для поступающих; 1 место-Среди лучших университетов которые принимают Common App в США, Лучшие университеты по преподаванию экологии и психологии в США; 94 000$-В среднем зарабатывают выпускники через 6 лет; 67%-Студентов университета получают финансовую помощь'," +
                "'Arts: Drama and Theatre Production, Film and Video Studies, Studio Arts; Humanities: Arican American Studies, Anthropology, Archaeology, Art History, Asian Studies, Cognitive Science, Communications, Economics, English; Science, Technology, and Math: Bioengineering and Biomedical EngineeringBiologyChemical EngineeringChemistryCivil EngineeringComputational and Applied MathematicsComputer Science'," +
                "'Мотивационное письмо Common App (250-650 слов), 7 коротких ответов на поставленные вопросы (10-50 слов), 3 мини-эссе на поставленные вопросы (100-250 слов). 1 секция для того, чтобы расписать вашу прошлую деятельность, занятия, достижения и т.д. (50-150 слов) Стэнфорд требует результаты SAT/ACT, рекомендует подать результаты 2-х SAT Subjects, но не требует SAT optional essay или ACT writing section. Также Стэнфорд требует результаты TOEFL/IELTS. Минимальных проходных баллов нет, но средний балл поступивших был между 1390-1540 по SAT и 32-35 по ACT.'," +
                "'Стипендия на основе потребностей (Need-based)-Стэнфорд выделяет финансовую помощь для международных студентов на основе их потребностей.'," +
                "'+1 650-723-2300; admission@stanford.edu; 450 Serra Mall; Stanford, CA 94305; stanford.edu')";
        db.execSQL(sql);
        sql="INSERT INTO "+MyDataProvider.TABLE_UNIVERSITIES+"("
                +MyDataProvider.KEY_UNIVER_IMG+","
                +MyDataProvider.KEY_UNIVER_NAME+","
                +MyDataProvider.KEY_UNIVER_NAMERUS+","
                +MyDataProvider.KEY_UNIVER_DESCRIPTION+","
                +MyDataProvider.KEY_UNIVER_COUNTRY+","
                +MyDataProvider.KEY_UNIVER_PRICE+","
                +MyDataProvider.KEY_UNIVER_FACTS+","
                +MyDataProvider.KEY_UNIVER_FACULTIES+","
                +MyDataProvider.KEY_UNIVER_REQUIREMENTS+","
                +MyDataProvider.KEY_UNIVER_GRANTS+","
                +MyDataProvider.KEY_UNIVER_CONTACTS
                +") VALUES('https://bluntforcetruth.com/wp-content/uploads/2017/09/yale-university-image-1-200x200.jpg'," +
                "'Yale'," +
                "'Йельский Университет'," +
                "'Йель является элитным частным университетом, расположенным в Нью-Хейвен, штат Коннектикут. Приемная комиссия данного университета крайне требовательна, что подтверждается показателем приема равным ~3-4% относительно международных абитуриентов. Популярные специальности включают историю, политологию и экономику.'," +
                "'USA'," +
                "'Все расходы/Только обучение: 78 725$/57 700$'," +
                "'2 января-Дедлайн подачи документов для поступающих; 1 место-Среди лучших профессоров в университетах. Лучшие университеты по истории, обществознанию и обществоведению в США; 83 200$-В среднем зарабатывают выпускники через 6 лет; 59%-Студентов университета получают финансовую помощь'," +
                "'Arts: Arts, Drama and Theatre Production, Film and Video Studies; Humanities: African American Studies, African Studies, Anthropology, Archaeology, Art History, Asian Studies, Cognitive Science, East Asian Languages and Literatures; Science, Technology, and Math: ArchitectureAstronomy and AstrophysicsBiochemistry and Molecular BiologyBioengineering and Biomedical EngineeringCellular BiologyChemical EngineeringChemistryComputational and Applied Mathematics'," +
                "'Мотивационное письмо Common App (250-650 слов), 3 мини-эссе на поставленные вопросы (1-250 слов), 4 вопроса с краткими ответами (~35 слов), секции для того, чтобы расписать вашу прошлую деятельность, занятия, достижения и т.д. (50-150 слов). Йель требует результаты SAT/ACT, рекомендует подать результаты 2-х SAT Subjects, но не требует SAT optional essay или ACT writing section. Также Йель требует результаты TOEFL/IELTS. Минимальных проходных баллов нет, но средний балл поступивших был между 1460-1580 по SAT и 32-35 по ACT. Минимальный проходной балл по IELTS равен 7.0.'," +
                "'Йельский Университет выделяет полный грант на основе потребностей международного студента (Need-based)'," +
                "'+1 203-432-9300; student.questions@yale.edu; 38 Hillhouse Ave; New Haven, CT 06511; admissions.yale.edu')";
        db.execSQL(sql);

        sql="INSERT INTO "+MyDataProvider.TABLE_UNIVERSITIES+"("
                +MyDataProvider.KEY_UNIVER_IMG+","
                +MyDataProvider.KEY_UNIVER_NAME+","
                +MyDataProvider.KEY_UNIVER_NAMERUS+","
                +MyDataProvider.KEY_UNIVER_DESCRIPTION+","
                +MyDataProvider.KEY_UNIVER_COUNTRY+","
                +MyDataProvider.KEY_UNIVER_PRICE+","
                +MyDataProvider.KEY_UNIVER_FACTS+","
                +MyDataProvider.KEY_UNIVER_FACULTIES+","
                +MyDataProvider.KEY_UNIVER_REQUIREMENTS+","
                +MyDataProvider.KEY_UNIVER_GRANTS+","
                +MyDataProvider.KEY_UNIVER_CONTACTS
                +") VALUES('https://4.bp.blogspot.com/-h9UCW_oCmek/W8HB9ODwEwI/AAAAAAAAAXo/6OaqPBaSxZcsScDVM7-hIvy8F56DVDacgCK4BGAYYCw/s200/unnamed.jpg'," +
                "'NYU'," +
                "'Нью-Йоркский Университет'," +
                "'Нью-Йорк является высокорейтинговым частным университетом, расположенным в Нью-Йорке, штат Нью-Йорк. Приемная комиссия данного университета требовательна, что подтверждается показателем приема равным ~11-13% относительно международных абитуриентов. Популярные специальности включают экономику и бизнес.'," +
                "'USA'," +
                "'Все расходы/Только обучение: 76 612$/53 308$'," +
                "'1 января-Дедлайн подачи документов для поступающих; 3 место-Среди лучших университетов по преподаванию информатики в США; 61 900$-В среднем зарабатывают выпускники через 6 лет; 54%-Студентов университета получают финансовую помощь'," +
                "'Мотивационное письмо Common App (250-650 слов), 1 эссе на поставленный вопрос (1-400 слов), секции для того, чтобы расписать вашу прошлую деятельность, занятия, достижения и т.д (50-150 слов). Нью-Йорк требует подать результаты SAT и SAT Subjects, но не требует SAT optional essay или требует ACT и ACT writing section. Нью-Йорк требует результаты TOEFL/IELTS. Минимальных проходных баллов нет, но средний балл поступивших был между 1290-1490 по SAT и 29-33 по ACT. Минимальный проходной балл по IELTS равен 7.5.'," +
                "'Arts: Animation, Video Graphics and Special Effects, Cinematography and Video Production, Dance, Drama and Theatre Production, Music Performance, Music Technology, Music Theory and Composition, Performing Arts, Photography, Studio Arts; Science, Technology, and Math: Biochemistry and Molecular Biology, Biology, Chemical Engineering, Chemistry, Civil Engineering, Computational and Applied Mathematics, Computer and Information Sciences; Humanities: Anthropology, Archaeology, Art History, Asian Studies, Clinical Psychology, Digital Communication and Media/Multimedia, Economics'," +
                "'Гранты на основе потребностей-Выдаются в случае подачи заявки на финансовую помощь через Common App'," +
                "'+1 212-998-4500; admissions@nyu.edu; 70 Washington Sq South; New York, NY 10012; nyu.edu')";
        db.execSQL(sql);

        sql="INSERT INTO "+MyDataProvider.TABLE_UNIVERSITIES+"("
                +MyDataProvider.KEY_UNIVER_IMG+","
                +MyDataProvider.KEY_UNIVER_NAME+","
                +MyDataProvider.KEY_UNIVER_NAMERUS+","
                +MyDataProvider.KEY_UNIVER_DESCRIPTION+","
                +MyDataProvider.KEY_UNIVER_COUNTRY+","
                +MyDataProvider.KEY_UNIVER_PRICE+","
                +MyDataProvider.KEY_UNIVER_FACTS+","
                +MyDataProvider.KEY_UNIVER_FACULTIES+","
                +MyDataProvider.KEY_UNIVER_REQUIREMENTS+","
                +MyDataProvider.KEY_UNIVER_GRANTS+","
                +MyDataProvider.KEY_UNIVER_CONTACTS
                +") VALUES('https://sun9-31.userapi.com/c856132/v856132496/f2312/5hB9bILG_y8.jpg?ava=1'," +
                "'KazNU'," +
                "'Казахский Национальный Университет им. аль-Фараби'," +
                "'КазНУ – это ведущий национальный университет, первым прошедший государственную аттестацию и подтвердивший право на осуществление образовательной деятельности по всем специальностям и уровням. По итогам исследования международного рейтингового агентства QS (Великобритания), КазНУ в 2020 году занимает 207 место среди лучших ВУЗ-ов мира'," +
                "'KZ'," +
                "'Стоимость обучения в КазНУ: 1. Бакалавриат - 1 406 000 тенге (3 700 долларов США); 2. Магистратура - 2 347 000 тенге (6 143 долларов США); 3. Докторантура - 2 804 000 тенге (7 340 долларов США)'," +
                "'1934-Дата основания университета; 1 место-Среди всех многопрофильных ВУЗ-ов РК'," +
                "'Наличие аттестата о среднем образовании/диплома об окончании организаций технического и профессионального образования; Наличие результата Единого Национального Тестирования (ЕНТ); Наличие медицинского освидетельствования'," +
                "'Факультет филологии и мировых языков; Факультет химии и химической технологии; Факультет биологии и биотехнологии; Физико-технический факультет; Механико-математический факультет; Факультет журналистики; Факультет истории, археологии и этнологии; Факультет географии и природопользования; Факультет философии и политологии; Высшая школа экономики и бизнеса; Юридический факультет; Факультет востоковедения; Факультет международных отношений; Медицинский факультет'," +
                "'Информация не предоставлена'," +
                "'Республика Казахстан, 050040, Алматы, пр.аль-Фараби 71; Phone: +7(727)221-10-00; E-mail: info@kaznu.kz; https://www.kaznu.kz/ru')";
        db.execSQL(sql);

        sql="INSERT INTO "+MyDataProvider.TABLE_UNIVERSITIES+"("
                +MyDataProvider.KEY_UNIVER_IMG+","
                +MyDataProvider.KEY_UNIVER_NAME+","
                +MyDataProvider.KEY_UNIVER_NAMERUS+","
                +MyDataProvider.KEY_UNIVER_DESCRIPTION+","
                +MyDataProvider.KEY_UNIVER_COUNTRY+","
                +MyDataProvider.KEY_UNIVER_PRICE+","
                +MyDataProvider.KEY_UNIVER_FACTS+","
                +MyDataProvider.KEY_UNIVER_FACULTIES+","
                +MyDataProvider.KEY_UNIVER_REQUIREMENTS+","
                +MyDataProvider.KEY_UNIVER_GRANTS+","
                +MyDataProvider.KEY_UNIVER_CONTACTS
                +") VALUES('http://s1.fotokto.ru/photo/large/522/5227893.jpg'," +
                "'KBTU'," +
                "'Казахстанско-Британский Технический Университет'," +
                "'КБТУ - это казахстанский ВУЗ, расположенный в городе Алматы, ориентирован на подготовку профессиональных кадров для самых ведущих секторов экономики: нефтегазового, информационного, финансового. Также КБТУ является участником программ обмена преподавателями и студентами, в том числе и \"Болашак\".'," +
                "'KZ'," +
                "'Стоимость обучения в КБТУ: от 1,45 млн KZT до 2 млн KZT (Магистратура до 3,5 млн KZT)'," +
                "'2001-Дата основания университета; 2 место-Среди всех технических ВУЗ-ов РК; Общежитие-Есть'," +
                "'Наличие аттестата о среднем образовании/диплома об окончании организаций технического и профессионального образования; Наличие результата Единого Национального Тестирования (ЕНТ); Наличие медицинского освидетельствования'," +
                "'Факультет информационных технологий; Факультет энергетики и нефтегазовой индустрии; Факультет геологии и геологоразведки; Научно-образовательный центр математики и кибернетики; Казахстанская Морская Академия; Научно-образовательный центр химической инженерии; Международная школа экономики; Бизнес школа'," +
                "'Информация не предоставлена'," +
                "'Республика Казахстан, 050000, Алматы, ул. Толе би, 59; Call-центр: +7 (702) 245-27-26; E-mail: info@kbtu.kz; https://kbtu.kz/kz/')";
        db.execSQL(sql);

        sql="INSERT INTO "+MyDataProvider.TABLE_UNIVERSITIES+"("
                +MyDataProvider.KEY_UNIVER_IMG+","
                +MyDataProvider.KEY_UNIVER_NAME+","
                +MyDataProvider.KEY_UNIVER_NAMERUS+","
                +MyDataProvider.KEY_UNIVER_DESCRIPTION+","
                +MyDataProvider.KEY_UNIVER_COUNTRY+","
                +MyDataProvider.KEY_UNIVER_PRICE+","
                +MyDataProvider.KEY_UNIVER_FACTS+","
                +MyDataProvider.KEY_UNIVER_FACULTIES+","
                +MyDataProvider.KEY_UNIVER_REQUIREMENTS+","
                +MyDataProvider.KEY_UNIVER_GRANTS+","
                +MyDataProvider.KEY_UNIVER_CONTACTS
                +") VALUES('https://pp.userapi.com/c847221/v847221251/10885a/LgPt27WpMPo.jpg?ava=1'," +
                "'МУИТ'," +
                "'Международный Университет Информационных Технологий'," +
                "'МУИТ - университет, расположенный в Бостандыкском районе города Алматы, Казахстан. Является одним из ведущих университетов Казахстана в сфере ИТ. МУИТ формирует специалистов нового поколения со знаниями не только отраслевых технологий, но и передового менеджмента, экономики, коммуникативных навыков с углубленным владением английским языком.'," +
                "'KZ'," +
                "'Стоимость обучения в МУИТ: от 900 000 KZT.'," +
                "'2009-Дата основания университета; 7 место-Среди всех технических ВУЗ-ов РК'," +
                "'Наличие аттестата о среднем образовании/диплома об окончании организаций технического и профессионального образования; Наличие результата Единого Национального Тестирования (ЕНТ); Наличие медицинского освидетельствования'," +
                "'Факультет менеджмента и управления; Факультет журналистики и репортерского дела; Факультет информационных технологий; Факультет информационной безопасности; Факультет коммуникаций и коммуникационных технологий; Факультет подготовки учителей информатики/физики'," +
                "'Информация не предоставлена'," +
                "'Республика Казахстан, 050040, Алматы, ул. Манаса, 8; Call-центр: +7(727)244-83-86; E-mail: info@iitu.kz; https://www.iitu.kz/ru/')";
        db.execSQL(sql);

        sql="INSERT INTO "+MyDataProvider.TABLE_UNIVERSITIES+"("
                +MyDataProvider.KEY_UNIVER_IMG+","
                +MyDataProvider.KEY_UNIVER_NAME+","
                +MyDataProvider.KEY_UNIVER_NAMERUS+","
                +MyDataProvider.KEY_UNIVER_DESCRIPTION+","
                +MyDataProvider.KEY_UNIVER_COUNTRY+","
                +MyDataProvider.KEY_UNIVER_PRICE+","
                +MyDataProvider.KEY_UNIVER_FACTS+","
                +MyDataProvider.KEY_UNIVER_FACULTIES+","
                +MyDataProvider.KEY_UNIVER_REQUIREMENTS+","
                +MyDataProvider.KEY_UNIVER_GRANTS+","
                +MyDataProvider.KEY_UNIVER_CONTACTS
                +") VALUES('https://media.licdn.com/dms/image/C4E0BAQEdsEsNCMzmWw/company-logo_200_200/0?e=2159024400&v=beta&t=tbVFNW0GmakUHCDpck_iU5CDRrNeN2TYgaZ_w1v8X2c'," +
                "'KIMEP'," +
                "'Казахстанский Институт Менеджмента'," +
                "'КИМЭП - это частно-государственный вуз, расположенный в городе Алматы, Казахстан. Осуществляет деятельность в сфере высшего образования в соответствии с кредитной американской системой обучения. Занятия в основном проводятся на английском языке'," +
                "'KZ'," +
                "'Стоимость обучения в КИМЭП-е: около 2 млн KZT ежегодно'," +
                "'1992-Дата основания университета; 1 место-Среди всех гуманитарно-экономических ВУЗ-ов РК; Общежитие-Есть'," +
                "'Наличие аттестата о среднем образовании/диплома об окончании организаций технического и профессионального образования; Наличие результата Единого Национального Тестирования (ЕНТ); Наличие медицинского освидетельствования'," +
                "'Факультет бизнеса им. Бэнга; Факультет социальных наук; Школа права; Факультет образования и гуманитарных наук'," +
                "'Информация не предоставлена'," +
                "'Республика Казахстан, 050000, Алматы, пр. Абая, 2; Call-центр: +7(727)270-43-14; E-mail: info@kimep.kz; https://kimep.kz/ru/')";
        db.execSQL(sql);

        sql="INSERT INTO "+MyDataProvider.TABLE_UNIVERSITIES+"("
                +MyDataProvider.KEY_UNIVER_IMG+","
                +MyDataProvider.KEY_UNIVER_NAME+","
                +MyDataProvider.KEY_UNIVER_NAMERUS+","
                +MyDataProvider.KEY_UNIVER_DESCRIPTION+","
                +MyDataProvider.KEY_UNIVER_COUNTRY+","
                +MyDataProvider.KEY_UNIVER_PRICE+","
                +MyDataProvider.KEY_UNIVER_FACTS+","
                +MyDataProvider.KEY_UNIVER_FACULTIES+","
                +MyDataProvider.KEY_UNIVER_REQUIREMENTS+","
                +MyDataProvider.KEY_UNIVER_GRANTS+","
                +MyDataProvider.KEY_UNIVER_CONTACTS
                +") VALUES('https://sun9-12.userapi.com/c638325/v638325113/15955/pwJYH3s8Dvc.jpg?ava=1'," +
                "'NarXoz'," +
                "'Университет Нархоз'," +
                "'Университет Нархоз — частный университет в Казахстане, осуществляющий обучение в области экономики, бизнеса, финансов и права, расположенный в городе Алматы. Университет является членом крупных международных ассоциаций ACPHA, РАБО, BMDA и активно сотрудничает с British Council, American Council, Bolashak.'," +
                "'KZ'," +
                "'Стоимость обучения в Нархозе: Нет официальной информации.'," +
                "'1963-Дата основания университета; 2 место-Среди всех гуманитарно-экономических ВУЗ-ов РК'," +
                "'Наличие аттестата о среднем образовании/диплома об окончании организаций технического и профессионального образования; Наличие результата Единого Национального Тестирования (ЕНТ); Наличие медицинского освидетельствования'," +
                "'Программы двойного диплома; Факультет общества, технологий и экологии; Бизнес школа; Факультет права и государственного управления'," +
                "'Информация не предоставлена'," +
                "'Республика Казахстан, 050035, Алматы, ул. Жандосова 55; Call-центр: +7(727)346 6464; E-mail: info@narxoz.kz; https://narxoz.kz/')";
        db.execSQL(sql);

        sql="INSERT INTO "+MyDataProvider.TABLE_UNIVERSITIES+"("
                +MyDataProvider.KEY_UNIVER_IMG+","
                +MyDataProvider.KEY_UNIVER_NAME+","
                +MyDataProvider.KEY_UNIVER_NAMERUS+","
                +MyDataProvider.KEY_UNIVER_DESCRIPTION+","
                +MyDataProvider.KEY_UNIVER_COUNTRY+","
                +MyDataProvider.KEY_UNIVER_PRICE+","
                +MyDataProvider.KEY_UNIVER_FACTS+","
                +MyDataProvider.KEY_UNIVER_FACULTIES+","
                +MyDataProvider.KEY_UNIVER_REQUIREMENTS+","
                +MyDataProvider.KEY_UNIVER_GRANTS+","
                +MyDataProvider.KEY_UNIVER_CONTACTS
                +") VALUES('https://media-exp1.licdn.com/dms/image/C4D0BAQEr7NDNythPxw/company-logo_200_200/0?e=2159024400&v=beta&t=BtVgjcNCB54anDBMa9ELG4pYlJJbJWUWB191hjKZQKI'," +
                "'NYU AD'," +
                "'Нью-Йоркский Университет в Абу-Даби'," +
                "'NYU AD является филиалом всемирно известного Нью-Йоркского университета. Расположен NYU AD в Абу-Даби, ОАЭ. Приемная комиссия данного университета также требовательна, как и в самом Нью-Йоркском университете. Документы в NYU AD подаются через Common App.'," +
                "'World'," +
                "'Все расходы/Только обучение: 75 060$/50 686$'," +
                "'1 января-Дедлайн подачи документов для поступающих; 2010-Дата основания университета; 7 место-Рейтинг университета в ОАЭ; 97%-Выпускников университета находят работу в течении 6 месяцев'," +
                "'Common Application-Мотивационное письмо Common App (250-650 слов), 1 эссе на поставленный вопрос (1-400 слов), секции для того, чтобы расписать вашу прошлую деятельность, занятия, достижения и т.д (50-150 слов). Результаты стандартизированных тестов-NYU AD требует подать результаты SAT и SAT Subjects, но не требует SAT optional essay или требует ACT и ACT writing section. NYU AD требует результаты TOEFL/IELTS. Минимальный проходной балл по IELTS равен 7.5.'," +
                "'Информация не предоставлена'," +
                "'NYU AD предоставляет гранты на основе потребностей (Need-based) и заслуг (Merit-based) для международных студентов. Гранты на основе заслуг могут покрывать только до 50% от всей цены, тем временем как гранты на основе потребностей покрывают до 100% цены.'," +
                "'PO Box 129188, Saadiyat Island, Abu Dhabi, United Arab Emirates; Номер приемной комиссии: +971-2-628-5511; E-mail: nyuad.admissions@nyu.edu; https://nyuad.nyu.edu/en/')";
        db.execSQL(sql);

        sql="INSERT INTO "+MyDataProvider.TABLE_UNIVERSITIES+"("
                +MyDataProvider.KEY_UNIVER_IMG+","
                +MyDataProvider.KEY_UNIVER_NAME+","
                +MyDataProvider.KEY_UNIVER_NAMERUS+","
                +MyDataProvider.KEY_UNIVER_DESCRIPTION+","
                +MyDataProvider.KEY_UNIVER_COUNTRY+","
                +MyDataProvider.KEY_UNIVER_PRICE+","
                +MyDataProvider.KEY_UNIVER_FACTS+","
                +MyDataProvider.KEY_UNIVER_FACULTIES+","
                +MyDataProvider.KEY_UNIVER_REQUIREMENTS+","
                +MyDataProvider.KEY_UNIVER_GRANTS+","
                +MyDataProvider.KEY_UNIVER_CONTACTS
                +") VALUES('https://wwwfr.uni.lu/var/storage/images/fdef/actualites/luxembourg_and_singapore_discuss_legal_issues_in_finance/1163408-1-fre-FR/luxembourg_and_singapore_discuss_legal_issues_in_finance.jpg'," +
                "'Yale NUS'," +
                "'Йельский Университет в Сингапуре'," +
                "'NUS является филиалом всемирно известного Йельского университета. Расположен NUS в Сингапуре. Приемная комиссия данного университета также крайне требовательна, как и в самом Йельском университете, что подтверждается показателем приема равным ~5%. NUS, также как и Йель, принимает Common App.'," +
                "'World'," +
                "'Все расходы/Только обучение: 50 944$/30 638$'," +
                "'15 января-Дедлайн подачи документов для поступающих; 2011-Дата основания университета; 13 место-Рейтинг университета в Сингапуре; 95%-Выпускников университета находят работу в течении 6 месяцев'," +
                "'Common Application-Мотивационное письмо Common App (250-650 слов), 1 мини-эссе на поставленный вопрос (1-250 слов), 1 эссе на поставленный вопрос (1-500 слов), 5 вопросов с краткими ответами, секции для того, чтобы расписать вашу прошлую деятельность, занятия, достижения и т.д. (50-150 слов). Результаты стандартизированных тестов-NUS требует результаты SAT/ACT, рекомендует подать результаты 2-х SAT Subjects, но не требует SAT optional essay или ACT writing section. Также NUS требует результаты TOEFL/IELTS. Минимальный проходной балл по IELTS равен 7.0.'," +
                "'Информация не предоставлена'," +
                "'NUS является Need-blind университетом. Он выделяет полные гранты на основе потребностей всем поступившим международным студентам (Need-based).'," +
                "'20 College Ave West, #03-403, Singapore 138529; Номер приемной комиссии: 6601 1000; E-mail: admissions@yale-nus.edu.sg; https://www.yale-nus.edu.sg/')";
        db.execSQL(sql);

        sql="INSERT INTO "+MyDataProvider.TABLE_UNIVERSITIES+"("
                +MyDataProvider.KEY_UNIVER_IMG+","
                +MyDataProvider.KEY_UNIVER_NAME+","
                +MyDataProvider.KEY_UNIVER_NAMERUS+","
                +MyDataProvider.KEY_UNIVER_DESCRIPTION+","
                +MyDataProvider.KEY_UNIVER_COUNTRY+","
                +MyDataProvider.KEY_UNIVER_PRICE+","
                +MyDataProvider.KEY_UNIVER_FACTS+","
                +MyDataProvider.KEY_UNIVER_FACULTIES+","
                +MyDataProvider.KEY_UNIVER_REQUIREMENTS+","
                +MyDataProvider.KEY_UNIVER_GRANTS+","
                +MyDataProvider.KEY_UNIVER_CONTACTS
                +") VALUES('https://images.ru.prom.st/677884305_w200_h200_universitet-toronto.jpg'," +
                "'TorontoU'," +
                "'Университет Торонто'," +
                "'Университет Торонто - государственный исследовательский университет в Торонто, Канада. Основной акцент в университете уделяется на научно-исследовательскую работу. Университет считается некоммерческим, следовательно очень щедрым, выделяя большое количество грантов и стипендий студентам. Показатель приема в университета равен ~43%.'," +
                "'World'," +
                "'Все расходы/Только обучение: 73 776-48 690$/58 680-35 890$'," +
                "'15 января-Дедлайн подачи документов для поступающих; 1827-Дата основания университета; 29 место-Рейтинг университета в мире; 92%-Выпускников университета находят работу в течении 6 месяцев'," +
                "'Toronto Application-Мотивационное письмо, секции для того, чтобы расписать вашу прошлую деятельность, занятия, достижения и т.д. Результаты стандартизированных тестов-Торонто требует результаты SAT/ACT, рекомендует подать результаты SAT Subjects, но не требует SAT optional essay или ACT writing section. Также Торонто требует результаты TOEFL/IELTS. Минимальный проходной балл по IELTS равен 6.5, каждая секция>6.0.'," +
                "'Информация не предоставлена'," +
                "'Торонто выделяет полные и частичные гранты на основе потребностей (Need-based) и на основе заслуг (Merit-based) международным студентам.'," +
                "'27 Kings College Circle; Toronto, Ontario M5S 1A1 Canada; Номер приемной комиссии: 416-978-2190; E-mail: admissions.sgs@utoronto.ca; https://www.utoronto.ca/')";
        db.execSQL(sql);

        sql="INSERT INTO "+MyDataProvider.TABLE_UNIVERSITIES+"("
                +MyDataProvider.KEY_UNIVER_IMG+","
                +MyDataProvider.KEY_UNIVER_NAME+","
                +MyDataProvider.KEY_UNIVER_NAMERUS+","
                +MyDataProvider.KEY_UNIVER_DESCRIPTION+","
                +MyDataProvider.KEY_UNIVER_COUNTRY+","
                +MyDataProvider.KEY_UNIVER_PRICE+","
                +MyDataProvider.KEY_UNIVER_FACTS+","
                +MyDataProvider.KEY_UNIVER_FACULTIES+","
                +MyDataProvider.KEY_UNIVER_REQUIREMENTS+","
                +MyDataProvider.KEY_UNIVER_GRANTS+","
                +MyDataProvider.KEY_UNIVER_CONTACTS
                +") VALUES('https://pbs.twimg.com/profile_images/819835056139145216/2LYQz20F_200x200.jpg'," +
                "'LSE'," +
                "'Лондонская Школа Экономики'," +
                "'LSE является высокорейтинговым частным университетом, расположенным в Лондоне. Приемная комиссия данного университета требовательна, что подтверждается показателем приема равным ~8-9%. Популярные специальности включают экономику, бизнес и политологию.'," +
                "'World'," +
                "'Все расходы/Только обучение: 43 812$/26 273$'," +
                "'15 января-Дедлайн подачи документов для поступающих; 1895-Дата основания университета; 44 место-Рейтинг университета в мире; 84%-Выпускников университета находят работу в течении 6 месяцев'," +
                "'LSE Online Account-Мотивационное письмо LSE (1-2 страницы, до 1500 слов), которое должно отражать ответ на 6 поставленных вопросов, секция CV для того, чтобы расписать вашу прошлую деятельность, занятия, достижения и т.д. Результаты стандартизированных тестов-LSE требует подать результаты TOEFL/IELTS или пройти программу LSE LCP. Минимальные проходные баллы по данным тестам: 1. IELTS 7.0, Each section>6.5; 2. TOEFL 100, Listening/Speaking>22, Reading>23, Writing>24; 3. LSE LCP 70, Each section>65'," +
                "'Информация не предоставлена'," +
                "'Стипендия USS:Данный грант предоставляется международным студентам на основе их потребностей (Need-based), в размере от 7500$ до 18000$ ежегодно.'," +
                "'Houghton Street, London WC2A 2AE; Phone: +44 (0)20 7405 7686; E-mail: graduate.documents@lse.ac.uk.')";
        db.execSQL(sql);

        sql="INSERT INTO "+MyDataProvider.TABLE_UNIVERSITIES+"("
                +MyDataProvider.KEY_UNIVER_IMG+","
                +MyDataProvider.KEY_UNIVER_NAME+","
                +MyDataProvider.KEY_UNIVER_NAMERUS+","
                +MyDataProvider.KEY_UNIVER_DESCRIPTION+","
                +MyDataProvider.KEY_UNIVER_COUNTRY+","
                +MyDataProvider.KEY_UNIVER_PRICE+","
                +MyDataProvider.KEY_UNIVER_FACTS+","
                +MyDataProvider.KEY_UNIVER_FACULTIES+","
                +MyDataProvider.KEY_UNIVER_REQUIREMENTS+","
                +MyDataProvider.KEY_UNIVER_GRANTS+","
                +MyDataProvider.KEY_UNIVER_CONTACTS
                +") VALUES('https://media.licdn.com/dms/image/C4D0BAQGPTDsKKex5zg/company-logo_200_200/0?e=2159024400&v=beta&t=IWWLc8bIF2hfaA3rDQpJ2SbJGi5ZyvTRfVWr4lb2C0A'," +
                "'PolyU'," +
                "'Гонконгский Политехнический Университет'," +
                "'PolyU - общественный университет, расположенный в Гонконге, в районе Хунхам. Финансируется Гонконгским комитетом университетских грантов. Университет считается некоммерческим, следовательно очень щедрым, выделяя большое количество грантов и стипендий студентам. Приемная комиссия PolyU очень требовательна, что подтверждается показателем приема не превышающим 5%'," +
                "'World'," +
                "'Все расходы/Только обучение: 26 200$/18 000$'," +
                "'Rolling Admissions-Дедлайн подачи документов для поступающих; 1937-Дата основания университета; 91 место-Рейтинг университета в мире; 86%-Выпускников университета находят работу в течении 3 месяцев'," +
                "'PolyU Application-Выбор программы обучения, ваше резюме, 3 секции чтобы расписать вашу прошлую деятельность, занятия, достижения и т.д. Отдельная секция для того, чтобы расписать ваш опыт работы. Результаты стандартизированных тестов-PolyU запрашвает результаты любых экзаменов которые аккредитованы Гонконгом и Великобританией. Отдельно PolyU запрашивает результаты экзаменов английского языка как IELTS, TOEFL. Минимальный балл по IELTS должен быть больше 6.0/TOEFL больше 80.'," +
                "'Информация не предоставлена'," +
                "'Грант Full Ride-Данный грант предоставляется международным студентам на основе их заслуг в размере ~24 500$ ежегодно.'," +
                "'12/F, Li Ka Shing Tower, The Hong Kong Polytechnic University, Hung Hom, Kowloon; Phone: (852) 2333-0600; E-mail: ar.qvs@polyu.edu.hk')";
        db.execSQL(sql);

    }

    @Override
    public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {

        db.execSQL("DROP TABLE IF EXISTS " + MyDataProvider.TABLE_UNIVERSITIES);
        onCreate(db);
    }

    public ArrayList<HashMap<String, String>> GetUnivers(String country) {
//Метод извлечения данных по стране
        SQLiteDatabase db = this.getWritableDatabase();
        ArrayList<HashMap<String, String>> univerList = new ArrayList<>();
        String query = "SELECT img, name, namerus FROM " + MyDataProvider.TABLE_UNIVERSITIES + " WHERE "+MyDataProvider.KEY_UNIVER_COUNTRY+"=" + "'" + country + "'";//запрос
        Cursor cursor = db.rawQuery(query, null);
        while (cursor.moveToNext()) {
            HashMap<String, String> univer = new HashMap<>();
            univer.put("img", cursor.getString(cursor.getColumnIndex(MyDataProvider.KEY_UNIVER_IMG)));
            univer.put("name", cursor.getString(cursor.getColumnIndex(MyDataProvider.KEY_UNIVER_NAME)));
            univer.put("namerus", cursor.getString(cursor.getColumnIndex(MyDataProvider.KEY_UNIVER_NAMERUS)));
            univerList.add(univer);
        }//вывод данных
        return univerList;//возвращение данных
    }

    public Cursor alldata(String uname){
//Метод извлечения данных по названию универа
        SQLiteDatabase db= this.getWritableDatabase();
        Cursor cursor=db.rawQuery("SELECT * from "+MyDataProvider.TABLE_UNIVERSITIES+" WHERE "+MyDataProvider.KEY_UNIVER_NAME+"="+"'"+uname+"'",null);

        return cursor;

    }
}
MyDataProvider
package com.example.uniinfo;

public class MyDataProvider {//класс для предоставления данных для создания таблицы
    public static  final String TABLE_UNIVERSITIES="universities";

    public static  final String KEY_UNIVER_ID="_id";

    public static  final String KEY_UNIVER_IMG="img";
    public static  final String KEY_UNIVER_NAME="name";
    public static  final String KEY_UNIVER_NAMERUS="namerus";
    public static  final String KEY_UNIVER_DESCRIPTION="about";
    public static  final String KEY_UNIVER_COUNTRY="country";
    public static  final String KEY_UNIVER_PRICE="price";
    public static  final String KEY_UNIVER_FACTS="facts";
    public static  final String KEY_UNIVER_FACULTIES="faculties";
    public static  final String KEY_UNIVER_REQUIREMENTS="reqs";
    public static  final String KEY_UNIVER_GRANTS="grants";
    public static  final String KEY_UNIVER_CONTACTS="contacts";
}
