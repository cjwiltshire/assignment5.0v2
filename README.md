<?php
include 'top.php';


// Open CSV with account names

$debug = false;

if(isset($_GET["debug"])){
    $debug = true;
}

$myFileName="accounting";

$fileExt=".csv";

$filename = $myFileName . $fileExt;

if ($debug) {
    print "\n\n<p>filename is " . $filename;
}

$file=fopen($filename, "r");

// the variable $file will be empty or false if the file does not open
if($file){
    if ($debug) {
        print "<p>File Opened</p>\n";
    }
}

// the variable $file will be empty or false if the file does not open
if($file){
    
    if ($debug) {
        print "<p>Begin reading data into an array.</p>\n";
    }

    // This reads the first row, which in our case is the account names
    $headers[]=fgetcsv($file);
    
    if($debug) {
        print "<p>Finished reading headers.</p>\n";
        print "<p>My header array<p><pre> "; print_r($headers); print "</pre></p>";
    }
    // the while (similar to a for loop) loop keeps executing until we reach 
// the end of the file at which point it stops. the resulting variable 
// $records is an array with all our data.

    while(!feof($file)){
        $records[]=fgetcsv($file);
    }
    
    //closes the file
    fclose($file);
    
    if($debug) {
        print "<p>Finished reading data. File closed.</p>\n";
        print "<p>My data array<p><pre> "; print_r($records); print "</pre></p>";
    }
} // ends if file was opened

// display the data
print "<ol>";
foreach ($records as $oneRecord) {
    if ($oneRecord[0] != "") {  //the eof would be a "" 
    print $records;
    print "\n\t</li>";
    }
}

print "</ol>";

if ($debug) {
    print "<p>End of processing.</p>\n";
}
//%^%^%^%^%^%^%^%^%^%^%^%^%^%^%^%^%^%^%^%^%^%^%^%^%^%^%^%^%^%^%^%^%^%^%^%^%^%^%
//
// SECTION: 1 Initialize variables
//
// SECTION: 1a.
// variables for the classroom purposes to help find errors.

$debug = false;

if (isset($_GET["debug"])) { // ONLY do this in a classroom environment
    $debug = true;
}

if ($debug) {
    print "<p>DEBUG MODE IS ON</p>";
}

//%^%^%^%^%^%^%^%^%^%^%^%^%^%^%^%^%^%^%
//
// SECTION: 1b Security
//
// define security variable to be used in SECTION 2a.

$yourURL = $domain . $phpSelf;
 


//%^%^%^%^%^%^%^%^%^%^%^%^%^%^%^%^%^%^%
//
// SECTION: 1c form variables
//
// Initialize variables one for each form element
// in the order they appear on the form



//%^%^%^%^%^%^%^%^%^%^%^%^%^%^%^%^%^%^%
//
// SECTION: 1d form error flags
//
// Initialize Error Flags one for each form element we validate
// in the order they appear in section 1c.
$cashERROR = false;
$accountERROR = false;



//%^%^%^%^%^%^%^%^%^%^%^%^%^%^%^%^%^%^%
//
// SECTION: 1e misc variables
//
// 

// create array to hold error messages filled (if any) in 2d displayed in 3c.
$errorMsg = array();
// array used to hold form values that will be written to a CSV file
$dataRecord = array();
$mailed=false; // have we mailed the information to the user?



//@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
//
// SECTION: 2 Process for when the form is submitted
//
if (isset($_POST["btnSubmit"])) {
    
    //@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
    //
    // SECTION: 2a Security
    // 
    if (!securityCheck(true)) {
        $msg = "<p>Sorry you cannot access this page. ";
        $msg.= "Security breach detected and reported</p>";
        die($msg);
    }
        
        
        
    
    
    //@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
    //
    // SECTION: 2b Sanitize (clean) data 
    // remove any potential JavaScript or html code from users input on the
    // form. Note it is best to follow the same order as declared in section 1c.
    
    
    
    $records = htmlentities($_POST["txtComments"], ENT_QUOTES, "UTF-8");
    $dataRecord[] = $comments;
    
   

    
    
    




    //@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
    //
    // SECTION: 2c Validation
    //
    // Validation section. Check each value for possible errors, empty or
    // not what we expect. You will need an IF block for each element you will
    // check (see above section 1c and 1d). The if blocks should also be in the
    // order that the elements appear on your form so that the error messages
    // will be in the order they appear. errorMsg will be displayed on the form
    // see section 3b. The error flag ($emailERROR) will be used in section 3c.
    
        
        
    
        
    if ($records == "") {
        $errorMsg[] = "Please enter 0 for none";
        $accountERROR = true;
    }
  
    if ($comments == "") {
        $errorMsg[] = "Throw something in the box";
        $commentsERROR = true;
    }
    
    
}
        
        
        
    
        
        
    
    



    //@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
    //
    // SECTION: 2d Process Form - Passed Validation
    //
    // Process for when the form passes validation (the errorMsg array is empty)
    //
    if (!$errorMsg) {
        if ($debug) {
        print "<p>Form is valid</p>";
    }




    //@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
        //
        // SECTION: 2e Save Data
        //
        // This block saves the data to a CSV file.
        $fileExt = ".csv";
        $myFileName = "data/registration";
        $filename = $myFileName . $fileExt;
        if ($debug) {
        print "\n\n<p>filename is " . $filename;
    }
    // now we just open the file for append
        $file = fopen($filename, 'a');
        // write the forms informations
        fputcsv($file, $dataRecord);
        // close the file
        fclose($file);
        
        
        
        
            
        
        
        
        
        
        
        







        //@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
        //
        // SECTION: 2f Create message
        //
        // 
        // build a message to display on the screen in section 3a and to mail
        // to the person filling out the form (section 2g).
        $message = '<h2>Your information.</h2>';
        foreach ($_POST as $key => $value) {
            $message .= "<p>";
            $camelCase = preg_split('/(?=[A-Z])/', substr($key, 3));
            foreach ($camelCase as $one) {
                $message .= $one . " ";
            }
            $message .= " = " . htmlentities($value, ENT_QUOTES, "UTF-8") . "</p>";
        }

        
        
            
            
            
                
            
            
        
        






        //@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
        //
        // SECTION: 2g Mail to user
        //
        // Process for mailing a message which contains the forms data
        // the message was built in section 2f.
        $to = $email; // the person who filled out the form
        $cc = "";
        $bcc = "";
        $from = "WRONG site <noreply@yoursite.com>";
        // subject of mail should make sense to your form
        $todaysDate = strftime("%x");
        $subject = "Research Study: " . $todaysDate;
        $mailed = sendMail($to, $cc, $bcc, $from, $subject, $message);
        
        
        
        
        
        
        
        
    
    


    } // end form is valid
 // ends if form was submitted.

//#############################################################################
//
// SECTION 3 Display Form
//
?>

<article id="main">

    <?php
    //####################################
    //
    // SECTION 3a.
    //
 // If its the first time coming to the form or there are errors we are going
    // to display the form.
    if (isset($_POST["btnSubmit"]) AND empty($errorMsg)) { // closing of if marked with: end body submit
        print "<h1>Your Request has ";
        
        if (!$mailed) {
            print "not ";
        }   
        
        print "been processed</h1>";
        
        
            
        print "<p>A copy of this message has ";
        if (!$mailed) {
            print "not ";
        }
        print "been sent</p>";
        print "<p>To: " . $email . "</p>";
        print "<p>Mail Message:</p>";
        print $message;
        
        
        
        
    } else {

    

        //####################################
        //
        // SECTION 3b Error Messages
        //
        // 
        if ($errorMsg) {
            print '<div id="errors">';
            print "<ol>\n";
            foreach ($errorMsg as $err) {
                print "<li>" . $err . "</li>\n";
            }
            print "</ol>\n";
            print '</div>';
        }
            
            
            
                
            
            
            
        
        


        //####################################
        //
        // SECTION 3c html Form
        //
        /* Display the HTML form. note that the action is to this same page. $phpSelf
          is defined in top.php
         NOTE the line:
          value="<?php print $email; ?>
          this makes the form sticky by displaying either the initial default value (line 35)
          or the value they typed in (line 84)
          NOTE this line:
          <?php if($emailERROR) print 'class="mistake"'; ?>
          this prints out a css class so that we can highlight the background etc. to
          make it stand out that a mistake happened here.
             
          
          
         
        */
        ?>

        <form action="<?php print $phpSelf; ?>"
              method="post"
              id="frmRegister">

            <fieldset class="wrapper">
                <legend>AccountNOW</legend>
                <p>The following form was created with small businesses in mind.</p>
                <p>Owning a small business is hard to do and paying an accountant to keep track of your expenses kills your profit margins.</p>
                <p>The following form was designed with you in mind!</p>
                <fieldset class="wrapperTwo">
                    <legend>Please complete the following form. Do not leave text boxes blank. Use 0 to indicate an irrelevant account and - to indicate negative balances. Amounts owed should be expressed as positives.</legend>

                    <fieldset class="accounts">
                        <legend>Business Information</legend>
                        <fieldset class="assets">
                            <legend>Assets</legend>
                        <label for="cash" class="required">Cash
                            <input type="number" id="cash" name="cash"
                                   value=""
                                   placeholder="0"
                                   <?php if ($cashERROR) print 'class="mistake"'; ?>
                                   onfocus="this.select()" 
                                   autofocus>
                        </label>               
                                                  
                        <label for="accountsreceivable" class="required">Accounts Receivable
                            <input type="number" id="accountsreceivable" name="accountsreceivable"
                                   value=""
                                   placeholder="0"
                                   <?php if ($cashERROR) print 'class="mistake"'; ?>
                                   onfocus="this.select()" 
                                   autofocus>
                        </label>
                        <label for="allowancefordoubtfulaccounts" class="required">Allowance for Doubtful Accounts
                            <input type="number" id="allowancefordoubtfulaccounts" name="allowancefordoubtfulaccounts"
                                   value=""
                                   placeholder="0"
                                   <?php if ($cashERROR) print 'class="mistake"'; ?>
                                   onfocus="this.select()" 
                                   autofocus>
                        </label>
                        <label for="inventory" class="required">Inventory 
                            <input type="number" id="inventory" name="inventory"
                                   value=""
                                   placeholder="0"
                                   <?php if ($cashERROR) print 'class="mistake"'; ?>
                                   onfocus="this.select()" 
                                   autofocus>
                        </label>
                        <label for="supplies" class="required">Supplies
                            <input type="number" id="supplies" name="supplies"
                                   value=""
                                   placeholder="0"
                                   <?php if ($cashERROR) print 'class="mistake"'; ?>
                                   onfocus="this.select()" 
                                   autofocus>
                        </label> 
                        <label for="prepaidrent" class="required">Prepaid Rent
                            <input type="number" id="prepaidrent" name="prepaidrent"
                                   value=""
                                   placeholder="0"
                                   <?php if ($cashERROR) print 'class="mistake"'; ?>
                                   onfocus="this.select()" 
                                   autofocus>
                        </label>
                        <label for="prepaidinsurance" class="required">Prepaid Insurance 
                            <input type="number" id="prepaidinsurance" name="prepaidinsurance"
                                   value=""
                                   placeholder="0"
                                   <?php if ($cashERROR) print 'class="mistake"'; ?>
                                   onfocus="this.select()" 
                                   autofocus>
                        </label>
                        <label for="land" class="required">Land
                            <input type="number" id="land" name="land"
                                   value=""
                                   placeholder="0"
                                   <?php if ($cashERROR) print 'class="mistake"'; ?>
                                   onfocus="this.select()" 
                                   autofocus>
                        </label>
                        <label for="buildings" class="required">Buildings 
                            <input type="number" id="buildings" name="buildings"
                                   value=""
                                   placeholder="0"
                                   <?php if ($cashERROR) print 'class="mistake"'; ?>
                                   onfocus="this.select()" 
                                   autofocus>
                        </label>
                        <label for="equipment" class="required">Equipment 
                            <input type="number" id="equipment" name="equipment"
                                   value=""
                                   placeholder="0"
                                   <?php if ($cashERROR) print 'class="mistake"'; ?>
                                   onfocus="this.select()" 
                                   autofocus>
                        </label>
                        <label for="vehicles" class="required">Vehicles 
                            <input type="number" id="vehicles" name="vehicles"
                                   value=""
                                   placeholder="0"
                                   <?php if ($cashERROR) print 'class="mistake"'; ?>
                                   onfocus="this.select()" 
                                   autofocus>
                        </label>    
                        <label for="accudepbuilding" class="required">Accumulated Depreciation Buildings 
                            <input type="number" id="accudepbuilding" name="accudepbuilding"
                                   value=""
                                   placeholder="0"
                                   <?php if ($cashERROR) print 'class="mistake"'; ?>
                                   onfocus="this.select()" 
                                   autofocus>
                        </label> 
                        <label for="accudepequipment" class="required">Accumulated Depreciation Equipment 
                            <input type="number" id="accudepequipment" name="accudepequipment"
                                   value=""
                                   placeholder="0"
                                   <?php if ($cashERROR) print 'class="mistake"'; ?>
                                   onfocus="this.select()" 
                                   autofocus>
                        </label>
                        <label for="accudepvehicles" class="required">Accumulated Depreciation Vehicles 
                            <input type="number" id="accudepvehicles" name="accudepvehicles"
                                   value=""
                                   placeholder="0"
                                   <?php if ($cashERROR) print 'class="mistake"'; ?>
                                   onfocus="this.select()" 
                                   autofocus>
                        </label>
                        </fieldset><!-- ends assets-->
                        <fieldset class="liabilities">
                        <label for="notespayable" class="required">Notes Payable 
                            <input type="number" id="notespayable" name="notespayable"
                                   value=""
                                   placeholder="0"
                                   <?php if ($cashERROR) print 'class="mistake"'; ?>
                                   onfocus="this.select()" 
                                   autofocus>
                        </label>
                        <label for="accountspayable" class="required">Accounts Payable 
                            <input type="number" id="accountspayable" name="accountspayable"
                                   value=""
                                   placeholder="0"
                                   <?php if ($cashERROR) print 'class="mistake"'; ?>
                                   onfocus="this.select()" 
                                   autofocus>
                        </label>
                        <label for="wagespayable" class="required">Wages Payable 
                            <input type="number" id="wagespayable" name="wagespayable"
                                   value=""
                                   placeholder="0"
                                   <?php if ($cashERROR) print 'class="mistake"'; ?>
                                   onfocus="this.select()" 
                                   autofocus>
                        </label>
                        <label for="interestpayable" class="required">Interest Payable 
                            <input type="number" id="interestpayable" name="interestpayable"
                                   value=""
                                   placeholder="0"
                                   <?php if ($cashERROR) print 'class="mistake"'; ?>
                                   onfocus="this.select()" 
                                   autofocus>
                        </label>
                        <label for="unearnedrevenue" class="required">Unearned Revenue 
                            <input type="number" id="uneaarnedrevenue" name="unearnedrevenue"
                                   value=""
                                   placeholder="0"
                                   <?php if ($cashERROR) print 'class="mistake"'; ?>
                                   onfocus="this.select()" 
                                   autofocus>
                        </label>
                        <label for="mortgagepayable" class="required">Mortgage Payable 
                            <input type="number" id="mortgagepayable" name="mortgagepayable"
                                   value=""
                                   placeholder="0"
                                   <?php if ($cashERROR) print 'class="mistake"'; ?>
                                   onfocus="this.select()" 
                                   autofocus>
                        </label>
                        <label for="bondspayable" class="required">Bonds Payable 
                            <input type="number" id="bondspayable" name="bondspayable"
                                   value=""
                                   placeholder="0"
                                   <?php if ($cashERROR) print 'class="mistake"'; ?>
                                   onfocus="this.select()" 
                                   autofocus>
                        </label>
                        <label for="discountonbondspayable" class="required">Discount on Bonds Payable 
                            <input type="number" id="discountonbondspayable" name="discountonbondspayable"
                                   value=""
                                   placeholder="0"
                                   <?php if ($cashERROR) print 'class="mistake"'; ?>
                                   onfocus="this.select()" 
                                   autofocus>
                        </label>
                        </fieldset> <!-- ends liabilities--> 
                        <fieldset class="stockholdersequity">
                        <label for="commonstock" class="required">Common Stock 
                            <input type="number" id="commonstock" name="commonstock"
                                   value=""
                                   placeholder="0"
                                   <?php if ($cashERROR) print 'class="mistake"'; ?>
                                   onfocus="this.select()" 
                                   autofocus>
                        </label>
                        <label for="retainedearnings" class="required">Retained Earnings                            
                            <input type="number" id="retainedearnings" name="retainedearnings"
                                   value=""
                                   placeholder="0"
                                   <?php if ($cashERROR) print 'class="mistake"'; ?>
                                   onfocus="this.select()" 
                                   autofocus>
                        </label>
                        <label for="treasurystock" class="required">Treasury Stock                            
                            <input type="number" id="treasurystock" name="treasurystock"
                                   value=""
                                   placeholder="0"
                                   <?php if ($cashERROR) print 'class="mistake"'; ?>
                                   onfocus="this.select()" 
                                   autofocus>
                        </label>
                        </fieldset> <!-- ends stockequity -->
                        <fieldset class="revenue">
                        <label for="salesrevenue" class="required">Sales Revenue                            
                            <input type="number" id="salesrevenue" name="salesrevenue"
                                   value=""
                                   placeholder="0"
                                   <?php if ($cashERROR) print 'class="mistake"'; ?>
                                   onfocus="this.select()" 
                                   autofocus>
                        </label>
                        <label for="servicerevenue" class="required">Service Revenue                            
                            <input type="number" id="servicerevenue" name="servicerevenue"
                                   value=""
                                   placeholder="0"
                                   <?php if ($cashERROR) print 'class="mistake"'; ?>
                                   onfocus="this.select()" 
                                   autofocus>
                        </label>
                        </fieldset><!--ends revenues-->
                        <fieldset class="expenses">
                        <label for="costofgoodssold" class="required">Cost of Goods Sold                          
                            <input type="number" id="costofgoodssold" name="costsofgoodssold"
                                   value=""
                                   placeholder="0"
                                   <?php if ($cashERROR) print 'class="mistake"'; ?>
                                   onfocus="this.select()" 
                                   autofocus>
                        </label>
                        <label for="payrollexpense" class="required">Payroll Expense                          
                            <input type="number" id="payrollexpense" name="payrollexpense"
                                   value=""
                                   placeholder="0"
                                   <?php if ($cashERROR) print 'class="mistake"'; ?>
                                   onfocus="this.select()" 
                                   autofocus>
                        </label>
                        <label for="marketingexpense" class="required">Marketing Expense                          
                            <input type="number" id="marketingexpense" name="marketingexpense"
                                   value=""
                                   placeholder="0"
                                   <?php if ($cashERROR) print 'class="mistake"'; ?>
                                   onfocus="this.select()" 
                                   autofocus>
                        </label> 
                        <label for="otherexpense" class="required">Other Expenses                          
                            <input type="number" id="otherexpense" name="otherexpense"
                                   value=""
                                   placeholder="0"
                                   <?php if ($cashERROR) print 'class="mistake"'; ?>
                                   onfocus="this.select()" 
                                   autofocus>
                        </label>
                            
                            
                        </fieldset><!--ends expenses-->
                        <fieldset><!--Extraordinary gains-->
                        <label for="saleofassets" class="required">Gain/Loss From Sale of Assets                
                            <input type="number" id="saleofassets" name="saleofassets="
                                   value=""
                                   placeholder="0"
                                   <?php if ($cashERROR) print 'class="mistake"'; ?>
                                   onfocus="this.select()" 
                                   autofocus>
                        </label>
                            
                        </fieldset><!--ends extraordinary gains-->
                        
                    </fieldset> <!-- ends accounts -->
                    
                </fieldset> <!-- ends wrapper Two -->
                <fieldset  class="textarea">					
                    <label for="txtComments" class="required">Comments</label>
                    <textarea id="txtComments" 
                            name="txtComments" 
                            tabindex="200"
                    <?php if ($commentsERROR) print 'class="mistake"'; ?>
                            onfocus="this.select()" 
                            style="width: 25em; height: 4em;" ><?php print $comments; ?></textarea>
                            <!-- NOTE: no blank spaces inside the text area -->
                </fieldset>
               

                <fieldset class="buttons">
                    <legend></legend>
                    <input type="submit" id="btnSubmit" name="btnSubmit" value="Register" tabindex="900" class="button">
                </fieldset> <!-- ends buttons -->
                
            </fieldset> <!-- Ends Wrapper -->
        </form>

  

</article>

<?php 

    }
    include "footer.php";
?>

</body>
</html>

/* CSS designed by Chris Wiltshire
to understand the comments like 900px / 960px
   read http://alistapart.com/article/responsive-web-design/
*/
/*****************     main layout section             ************************/
body{
    margin: 36px auto;
    width: 90%;
}

#page{
    margin: 0 auto 53px;
    width: 93.75%; /* 900px / 960px  */
    border: 1px solid black;
    border-radius: 15px;
}

#main{
    float: left;
    width: 40%;
    padding: 1%;
    
}
img{
    position: absolute; 
    top: 0;
    right: 0;
    width: 65px;
    height: 125px;
    border: 6px solid activeborder;
    overflow: scroll;
      
}
figcaption{
    float: right;
    position: absolute;
    top: 125px;
    right: 0;
      
}

#other{
    float: right;
    width: 40%; 
    padding: 1%;
    
}

/*****************     header section             *****************************/
header h1 {
    padding: 0.8em 5.33333333%;  /* 48px / 900px */
}


/*****************     navigation section         *****************************/
nav ol {
    padding-left: 5.33333333%;  /* 48px / 900px */
}
nav ol li{
    display: inline-block;
    list-style-type: none;
    padding-right: 3em;
}
nav ol li:hover{
    background-color: lightblue;
}


/*****************     article section             ****************************/

article{
    margin: 2px;
    padding: 2px;
    height: 400px;
    overflow: auto;
    background-color: lightgreen
}


article h1, article p, article table{
    padding: .2em 8.48056537% .2em 8.48056537%; /* 48px / 566px */
}


/*****************     aside section              *****************************/
aside {
    margin: 2px;
    padding: 2px;
    height: 400px;
    overflow: auto;
    background-color: lightskyblue
}
aside h2, aside p{
    padding: .2em 8.48056537% .2em 8.48056537%; /* 48px / 566px */
}


/*****************     footer section              ****************************/
footer{
    clear: both;
    padding: 0.8em 5.33333333%;  /* 48px / 900px */
}

/*****************     form section                ****************************/
#form article#main{
    float: none;
    margin: auto; /* because of the fixed width this centers the form */
    text-align: left;
    width: 600px; /* not repsonive but fixed */
    padding: 5px;
}

/* need to clear any floats that may exist */
fieldset{
    border: 1px solid #00498e;
    clear: both;  
}
/* just pushes content to left side of the box. */
fieldset.contact{
    float: left;
}
/* when you look at the labels this pushes then next to the input and right
aligns them. Display block means they are each on their own line. */
fieldset.contact label{
    display: block;
    text-align: right /* pushes the text to the right where they line up */
        /* the float left above pushes everything back to the left*/
}

fieldset.contact input{
    width: 15em; /* works with above to be the same size for the boxes */
}

fieldset.buttons{
    border: none;
}

fieldset.wrapper {
    border: 0;
    margin: auto;
}

fieldset.wrapper legend {
    color: #00498e;
    font-size: 1.2em;
    font-weight: bold;
    margin-top: 10px;
}

fieldset.wrapper > legend {
    /* i only want the first legend ie if it follows the class wrapper */
    font-size: 2em;
}

/***********************************************************************/

label {
    color: #005cb3;
    display: block;
    font-size: .9em;
    font-weight: bold;
}
