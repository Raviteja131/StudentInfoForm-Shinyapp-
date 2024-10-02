# StudentInfoForm-Shinyapp-  
StudentInfoForm(Shinyapp)

link:- https://ravitejamoningi.shinyapps.io/StudentInfoForm/  

Code:-

library(shiny)

# Define the UI
ui <- fluidPage(
  
  # Increase the page size with CSS
  tags$head(
    tags$style(HTML("
      body, html {
        height: 100%;
      }
      .container-fluid {
        min-height: 150vh; /* Increase vertical height */
      }
    "))
  ),
  
  # App title
  titlePanel("Student Registration Form"),
  
  # Layout with input fields
  fluidRow(
    column(12,  # Use full page width for the form
           textInput("first_name", "First Name", value = "", width = '50%'),
           textInput("last_name", "Last Name", value = "", width = '50%'),
           dateInput("dob", "Date of Birth", value = Sys.Date(), width = '50%'),
           selectInput("gender", "Gender", choices = c("Male", "Female", "Other"), width = '50%'),
           textAreaInput("address", "Address", value = "", rows = 3, width = '50%'),
           textInput("email", "Email ID", value = "", width = '50%'),
           textInput("phone", "Phone Number", value = "", width = '50%'),
           
           # Add the new "Message" field
           textAreaInput("message", "Message", value = "", rows = 3, width = '50%'),
           
           # Submission buttons (at the end of the form)
           br(),
           actionButton("register", "Submit Registration", class = "btn-primary"),
           actionButton("clear", "Clear Fields", class = "btn-secondary"),
           br(), br(),
           tags$h5("Submission Status:"),
           verbatimTextOutput("submit_status")
    )
  )
)

# Define the server logic
server <- function(input, output, session) {
  
  # Action when the Submit button is clicked
  observeEvent(input$register, {
    # Check if any required fields are empty
    if (input$first_name == "" || input$last_name == "" || input$email == "" || input$phone == "") {
      output$submit_status <- renderText("Please fill out all required fields.")
    } else {
      # Confirmation message
      output$submit_status <- renderText("Registration successful!")
      
      # Clear the form automatically after submission
      updateTextInput(session, "first_name", value = "")
      updateTextInput(session, "last_name", value = "")
      updateDateInput(session, "dob", value = Sys.Date())
      updateSelectInput(session, "gender", selected = "")
      updateTextAreaInput(session, "address", value = "")
      updateTextInput(session, "email", value = "")
      updateTextInput(session, "phone", value = "")
      updateTextAreaInput(session, "message", value = "")
    }
  })
  
  # Clear input fields when Clear button is clicked
  observeEvent(input$clear, {
    updateTextInput(session, "first_name", value = "")
    updateTextInput(session, "last_name", value = "")
    updateDateInput(session, "dob", value = Sys.Date())
    updateSelectInput(session, "gender", selected = "")
    updateTextAreaInput(session, "address", value = "")
    updateTextInput(session, "email", value = "")
    updateTextInput(session, "phone", value = "")
    updateTextAreaInput(session, "message", value = "")
    
    output$submit_status <- renderText("")
  })
}

# Run the Shiny app
shinyApp(ui = ui, server = server)


Description:-

This Shiny application implements a simple student registration form with various fields like First Name, Last Name, Date of Birth, Gender, Address, Email, Phone Number, and Message. It allows users to submit their information or reset the form.

UI Implementation
The UI is designed using fluidPage() which lays out the web page with a flexible design. The layout includes the following components:

Text Inputs (textInput()):

Used for entering First Name, Last Name, Email ID, and Phone Number.
Example: textInput("first_name", "First Name", value = "", width = '50%')
Date Input (dateInput()):

Allows the user to select a date for the Date of Birth field.
Example: dateInput("dob", "Date of Birth", value = Sys.Date(), width = '50%')
Select Input (selectInput()):

Used for selecting Gender with predefined choices (Male, Female, Other).
Example: selectInput("gender", "Gender", choices = c("Male", "Female", "Other"), width = '50%')
Text Area Input (textAreaInput()):

Used for entering longer text fields like Address and Message.
Example: textAreaInput("address", "Address", value = "", rows = 3, width = '50%')
Submit and Clear Buttons (actionButton()):

The form contains two action buttons. The Submit Registration button triggers form submission, while the Clear Fields button resets the form fields.
Example: actionButton("register", "Submit Registration", class = "btn-primary")
Verbatim Output (verbatimTextOutput()):

Displays the status of the form submission (successful or if fields are missing).
Example: verbatimTextOutput("submit_status")
