sdfgdgfd
plugins {
    id 'com.android.application'
    id 'org.jetbrains.kotlin.android'
}

android {
    compileSdk 32

    defaultConfig {
        applicationId "com.shashigroup.mvvmexample"
        minSdk 22
        targetSdk 32
        versionCode 1
        versionName "1.0"

        testInstrumentationRunner "androidx.test.runner.AndroidJUnitRunner"
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
    kotlinOptions {
        jvmTarget = '1.8'
    }
    dataBinding {
        enabled = true
    }

}

dependencies {

    implementation 'androidx.core:core-ktx:1.7.0'
    implementation 'androidx.appcompat:appcompat:1.4.2'
    implementation 'com.google.android.material:material:1.6.1'
    testImplementation 'junit:junit:4.13.2'
    androidTestImplementation 'androidx.test.ext:junit:1.1.3'
    androidTestImplementation 'androidx.test.espresso:espresso-core:3.4.0'
    implementation 'com.google.android.material:material:1.5.0-alpha02'


    implementation 'androidx.lifecycle:lifecycle-extensions:2.2.0'
    implementation "androidx.lifecycle:lifecycle-viewmodel-ktx:2.3.0-alpha06"
    implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-android:1.3.4'

    implementation 'com.google.android.material:material:1.2.1'
    //network perform
    implementation 'com.squareup.retrofit2:retrofit:2.9.0'
    implementation 'com.google.code.gson:gson:2.8.6'
    implementation 'com.squareup.retrofit2:converter-gson:2.9.0'
    implementation 'com.github.bumptech.glide:glide:4.12.0'
    annotationProcessor 'com.github.bumptech.glide:compiler:4.12.0'

}

package com.shashigroup.mvvmexample.login.model

class Login_Model {

    lateinit var email: String
    lateinit var password: String

    constructor(email: String, password: String) {
        this.email = email
        this.password = password
    }


    fun setemail(email: String) {
        this.email = email;
    }

    fun getemail(): String {
        return email
    }

    fun setpassword(password: String) {
        this.password = password;
    }

    fun getpassword(): String {
        return password
    }
    fun validationEmail(): Boolean {
        var value = getemail();
        if (value.isEmpty()) {
            return false
        } else {
            return android.util.Patterns.EMAIL_ADDRESS.matcher(value).matches()
        }
    }
    fun validationPassword(): Boolean {
        return getpassword().length == 0

    }
    fun validationPasswordLenght(): Boolean {
        return getpassword().length < 5

    }



}


package com.shashigroup.mvvmexample.login.viewmodel

import android.view.View
import androidx.lifecycle.MutableLiveData
import androidx.lifecycle.ViewModel
import com.shashigroup.mvvmexample.login.model.Login_Model





class LoginViewModel : ViewModel() {
    var email: MutableLiveData<String> = MutableLiveData()
    var password: MutableLiveData<String> = MutableLiveData()
    var userMutableLiveData: MutableLiveData<Login_Model> = MutableLiveData()


    init {
        email.postValue("")
        password.postValue("")
    }
    fun getUser(): MutableLiveData<Login_Model> {
        if (userMutableLiveData == null) {
            userMutableLiveData = MutableLiveData()
        }
        return userMutableLiveData
    }

    fun onClick(view: View?) {
        val loginUser = password.getValue()?.let { email.getValue()
            ?.let { it1 -> Login_Model(it1, it) } }
        userMutableLiveData.setValue(loginUser)
    }
}
    <?xml version="1.0" encoding="utf-8"?>
<layout>

    <data>

        <variable
            name="loginViewModel"
            type="com.shashigroup.mvvmexample.login.viewmodel.LoginViewModel" />
    </data>

    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">

        <EditText
            android:layout_width="fill_parent"
            android:layout_height="wrap_content"
            android:text="@={loginViewModel.email}" />

        <EditText
            android:layout_width="fill_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="16dp"
            android:text="@={loginViewModel.password}" />

        <Button
            android:onClick="@{(v) -> loginViewModel.onClick(v)}"

            android:layout_width="fill_parent"
            android:layout_height="wrap_content"
            android:text="OK" />
    </LinearLayout>
</layout>
