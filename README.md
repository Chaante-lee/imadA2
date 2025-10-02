this is my readme file for my imad assignment 2

my repo link - https://github.com/Chaante-lee/imadA2


youtube video link - 


report -




code

activity_main.xml

<?xml version="1.0" encoding="utf-8"?>
<ScrollView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:padding="16dp"
        android:orientation="vertical"
        android:gravity="center">

        <RelativeLayout
             android:layout_width="match_parent"
             android:layout_height="match_parent"
              android:background="@drawable/download" />

        <TextView
            android:id="@+id/tvQuestion"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textSize="22sp"
            android:textAlignment="center"
            android:padding="16dp" />

        <Button
            android:id="@+id/btnTrue"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="True"
            android:layout_marginTop="8dp" />

        <Button
            android:id="@+id/btnFalse"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="False"
            android:layout_marginTop="8dp" />

        <TextView
            android:id="@+id/tvFeedback"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textSize="18sp"
            android:padding="8dp"
            android:textStyle="italic" />

        <Button
            android:id="@+id/btnNext"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Next" />

        <Button
            android:id="@+id/btnStart"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Start Quiz" />

        <Button
            android:id="@+id/btnReview"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Review Answers" />

        <Button
            android:id="@+id/btnRestart"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Restart Quiz" />

        <Button
            android:id="@+id/btnEnd"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="End Quiz" />

        <ListView
            android:id="@+id/reviewList"
            android:layout_width="match_parent"
            android:layout_height="227dp" />

    </LinearLayout>
</ScrollView>

MainActivity.kt

package com.example.imada2 // name of package

import android.os.Bundle
import android.widget.*
import androidx.appcompat.app.AppCompatActivity //author z bulbulia

class MainActivity : AppCompatActivity() {

    data class Flashcard(val question: String, val correctAnswer: Boolean) //declaration author chaante lee pillay

    private val flashcards = listOf( //declared variables author chaante lee pillay
        Flashcard("Maths is fun :).", true),
        Flashcard("Lewis Hamilton won the world drivers championship last year 2024.", false),
        Flashcard("Kotlin is a programming language.", true),
        Flashcard("2 + 2 equals 5.", false),
        Flashcard("The Earth revolves around the Sun.", true)
    )

    private var currentIndex = 0
    private var score = 0
    private var state = State.WELCOME

    private lateinit var questionText: TextView // buttons to show if answer is correct and to choose your answers
    private lateinit var feedbackText: TextView
    private lateinit var trueBtn: Button
    private lateinit var falseBtn: Button
    private lateinit var nextBtn: Button
    private lateinit var startBtn: Button
    private lateinit var restartBtn: Button
    private lateinit var endBtn: Button
    private lateinit var reviewBtn: Button
    private lateinit var reviewList: ListView

    enum class State { //used to to define a set of named constants that represent distinct states, categories, or options
        WELCOME, QUIZ, SCORE, REVIEW
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main) //this links the layout author chaante lee pillay

        questionText = findViewById(R.id.tvQuestion) //here i declared more variables author chaante lee pillay
        feedbackText = findViewById(R.id.tvFeedback)
        trueBtn = findViewById(R.id.btnTrue)
        falseBtn = findViewById(R.id.btnFalse)
        nextBtn = findViewById(R.id.btnNext)
        startBtn = findViewById(R.id.btnStart)
        restartBtn = findViewById(R.id.btnRestart)
        endBtn = findViewById(R.id.btnEnd)
        reviewBtn = findViewById(R.id.btnReview)
        reviewList = findViewById(R.id.reviewList)

        startBtn.setOnClickListener { // start button author chaante lee pillay
            state = State.QUIZ
            startQuiz()
        }

        trueBtn.setOnClickListener { checkAnswer(true) } // checks if answer is true or false buttons author chaante lee pillay
        falseBtn.setOnClickListener { checkAnswer(false) }
        nextBtn.setOnClickListener { nextQuestion() }
        restartBtn.setOnClickListener {
            currentIndex = 0
            score = 0
            state = State.QUIZ
            startQuiz()
        }
        endBtn.setOnClickListener { finishAffinity() }
        reviewBtn.setOnClickListener { showReview() }

        showWelcome()
    }

    private fun showWelcome() { //welcome text shows on app author chaante lee pillay
        setAllGone()
        questionText.text = "Welcome to Flashcard Quiz!\nTest your knowledge with 5 questions."
        startBtn.visibility = Button.VISIBLE
    }

    private fun startQuiz() { // modifier used to restrict the access of a declaration author chaante lee pillay
        setAllGone()
        loadQuestion()
        trueBtn.visibility = Button.VISIBLE
        falseBtn.visibility = Button.VISIBLE
        nextBtn.visibility = Button.VISIBLE
    }

    private fun loadQuestion() { // modifier used to restrict the access of a declaration for the questions author chaante lee pillay
        setAllGone()
        val question = flashcards[currentIndex]
        questionText.text = question.question
        feedbackText.text = ""
        trueBtn.isEnabled = true
        falseBtn.isEnabled = true
        questionText.visibility = TextView.VISIBLE
        trueBtn.visibility = Button.VISIBLE
        falseBtn.visibility = Button.VISIBLE
        nextBtn.visibility = Button.VISIBLE
        feedbackText.visibility = TextView.VISIBLE
    }

    private fun checkAnswer(answer: Boolean) { // modifier used to restrict the access of a declaration for checking if the answers are correct
        val correct = flashcards[currentIndex].correctAnswer
        if (answer == correct) {
            feedbackText.text = "Correct! Well done!"
            score++
        } else {
            feedbackText.text = "Try again!"
        }
        trueBtn.isEnabled = false
        falseBtn.isEnabled = false
    }

    private fun nextQuestion() { // condition for next questions author chaante lee pillay
        if (currentIndex < flashcards.size - 1) {
            currentIndex++
            loadQuestion()
        } else {
            showScore()
        }
    }

    private fun showScore() { //condition for correct answers , what type of text is shown when correct and incorrect author chaante lee pillay
        state = State.SCORE
        setAllGone()
        questionText.text = "You got $score out of 5 correct."
        feedbackText.text = when {
            score == 5 -> "Amazing! Great job!"
            score >= 3 -> "Well done!"
            else -> "Keep practicing!"
        }
        questionText.visibility = TextView.VISIBLE
        feedbackText.visibility = TextView.VISIBLE
        reviewBtn.visibility = Button.VISIBLE
        restartBtn.visibility = Button.VISIBLE
        endBtn.visibility = Button.VISIBLE
    }

    private fun showReview() { // condition when reviewing flashcards chaante lee pillay
        state = State.REVIEW
        setAllGone()
        questionText.text = "Review Flashcards"
        questionText.visibility = TextView.VISIBLE
        reviewList.visibility = ListView.VISIBLE
        val items = flashcards.map {
            "${it.question}\nAnswer: ${if (it.correctAnswer) "True" else "False"}"
        }
        reviewList.adapter = ArrayAdapter(this, android.R.layout.simple_list_item_1, items)
        restartBtn.visibility = Button.VISIBLE
        endBtn.visibility = Button.VISIBLE
    }

    private fun setAllGone() { // condition choosing what to do when finished author chaante lee pillay
        listOf(
            questionText, feedbackText,
            trueBtn, falseBtn, nextBtn,
            startBtn, restartBtn, endBtn,
            reviewBtn, reviewList
        ).forEach { it.visibility = LinearLayout.GONE }
    }
}
<img width="808" height="683" alt="Screenshot 2025-10-02 182208" src="https://github.com/user-attachments/assets/0d76c74c-16b7-4489-b435-ae21500c6d5f" />


<img width="651" height="680" alt="Screenshot 2025-10-02 182225" src="https://github.com/user-attachments/assets/8af86d62-0541-4a13-ba5c-b2608e892c22" />


<img width="701" height="681" alt="Screenshot 2025-10-02 182257" src="https://github.com/user-attachments/assets/41eaefb4-708d-4b27-92f5-cb8d6f10abf9" />


<img width="666" height="671" alt="Screenshot 2025-10-02 182316" src="https://github.com/user-attachments/assets/03d404ad-b305-49f5-aac1-f8599d50d1bf" />


