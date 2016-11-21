<?php
	//for africastalking  api
	$phonenumber = $_POST['phoneNumber'];  
	$sessionID = $_POST['sessionId'];  
	$servicecode = $_POST['serviceCode'];  
	$ussdString = $_POST['text'];  
  
    
	$level = 0;  
  
	if($ussdString != ""){  
		$ussdString=  str_replace("#", "*", $ussdString);  
		$ussdString_explode = explode("*", $ussdString);  
		$level = count($ussdString_explode);  
	}
	
	if ($level == 0){  
		displaymenu();  
	} 
	
	function displaymenu(){  
		$ussd_text="CON infoshule. \n 1. Kiswahili \n 2. English \n";  
		ussd_proceed($ussd_text);  
	} 
	
	function mwanzo($exploded_text){  
		$ussd_text="CON infoshule. \n 1. Salio \n 2. Balance \n 3. Withdraw \n 4. Deposit \n";  
		ussd_proceed($ussd_text);  
	} 
	
	function ussd_proceed ($ussd_text){ 		
		echo $ussd_text;
		exit(0);  
	} 
	
	if ($level > 0 ){  
		switch ($ussdString_explode[0]) {  
			case 1:  
				kiswahili_language($ussdString_explode,$phonenumber);  
				break;  
			case 2:  
				english_language($ussdString_explode,$phonenumber);  
				break;
			default:
				echo "END Samahani umeingiza chaguo lisilo sahihi. Tafadhali jaribu tena baadae";
				break;
		}
	} 
	
  
	function kiswahili_language($details,$phone){  
		if (count($details)==1){  
			$ussd_text="CON <br/> Ingiza namba ya akaunti"; //enter account number
			ussd_proceed($ussd_text);  
		} 	
		else if (count($details)==2){  
			$ussd_text="CON <br/> Ingiza namba ya siri";  //enter password
			ussd_proceed($ussd_text);  
		}
		else if(count($details) == 3){  
			$username = $details[1];  
			$password = $details[2];    
				$acc = new Account();
					$h_pass = md5($password);
					$res= $acc::Authenticatestudent($username,$h_pass);
					if($res == true){
							$mwanzo = mwanzo($details);
						}
					}else{
						echo "END Taarifa ulizoingiza si sahihi"; //invalid details
					}
	}  

	function badili_neno_siri($details){
		if(count($details) == 1){
			$ussd_text = "Ingiza namba ya siri ya sasa"; //enter current password
			ussd_proceed($ussd_text);
		}
		if(count($details) == 2){
			$ussd_text = "Ingiza namba ya siri mpya"; //enter new password
			ussd_proceed($ussd_text);
		}
		if(count($details) == 3){
			$ussd_text = "Hakiki namba ya siri mpya"; //confirm new password
			ussd_proceed($ussd_text);
		}
		
		if(count($details) == 4){
			$current_pass = $details[1];
			$new_pass = $details[2];
			$confirm_pass = $details[3];
			
		}else{
			mwanzo();
		}
	}	
?>