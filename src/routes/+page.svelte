<script lang="ts">

	let cardName = "";
	let cardNumber = "";
	let cardMonth = "";
	let cardYear = "";
	let cardCVC = "";

	let cardNumberError: string | null = null;
	let dateError: string | null = null;
	let cvcError: string | null = null;

	let submitted = false;
	let formSubmitted = false; 

	function validateCardNumber() {
		const cleanedCardNumber = cardNumber.replace(/\s+/g, '');
		if (!/^\d{16}$/.test(cleanedCardNumber)) {
			cardNumberError = 'Wrong format. Numbers only (16 digits).';
		} else {
			cardNumberError = null;
		}
	}

	function validateExpirationDate() {
		if (!cardMonth || !cardYear) {
			dateError = "Expiration date can't be blank.";
		} else if (!/^\d{2}$/.test(cardMonth) || !/^\d{2}$/.test(cardYear)) {
			dateError = 'Enter a valid expiration date (MM/YY).';
		} else {
			dateError = null;
		}
	}

	function validateCVC() {
		if (!cardCVC || !/^\d{3}$/.test(cardCVC)) {
			cvcError = 'CVC must be 3 digits.';
		} else {
			cvcError = null;
		}
	}

	function handleInputChange() {
		if (submitted) {
			validateCardNumber();
			validateExpirationDate();
			validateCVC();
		}
	}

	function handleSubmit() {
		submitted = true;
		validateCardNumber();
		validateExpirationDate();
		validateCVC();

		if (!cardNumberError && !dateError && !cvcError) {
			formSubmitted = true;  
			
			
		} else {
			alert('Please fix the errors in the form.');
		}
	}
</script>


<section>
	<div class="left-side">
		<img src="/bg-main-desktop.png" alt="bg photo" class="bg" />
        <img src="/bg-main-mobile.png" alt="bg photo" class="bg-mobile" />
		<div class="cards">
			<div class="card-front-wrapper">
				<img src="/bg-card-front.png" alt="card front" class="card-front" />
				<div class="card-details">
					<img src="/card-logo.svg" alt="card logo" class="card-logo" />
			
					<div class="card-number">{cardNumber || "0000 0000 0000 0000"}</div>
					<div class="date-space">
						<div class="card-name">{cardName || "JANE APPLESEED"}</div>
						<div class="card-expiry">{cardMonth || "00"}/{cardYear || "00"}</div>
					</div>
					
				</div>
			</div>
			<div class="card-front-wrapper">
				<img src="/bg-card-back.png" alt="card back" class="card-back"/>
				<div class="card-details-back">
			
					<div class="card-number-back">{cardCVC || "000"}</div>
					
				</div>
			</div>
					

		</div>
	</div>
	
    {#if !submitted}
	<div class="card"> 
		<label for="name">CARDHOLDER NAME</label>
		<input type="text" placeholder="e.g. Janee Appleseed" bind:value={cardName}  />
		<label for ="cardNo">CARD NUMBER</label>
		<input type="text" placeholder="e.g. 1234 5678 9123 0000" bind:value={cardNumber} class:error={cardNumberError} on:input={handleInputChange}/>
		{#if cardNumberError}
			<p class="error">{cardNumberError}</p>
		{/if}
		<div class="date-cvc">
			<div>
				<label for="date">EXP. DATE (MM/YY) </label>
				<div class="date-fields">
					<input type="text" placeholder="MM" bind:value={cardMonth} maxlength="2" class:error={dateError} on:input={handleInputChange}  />
					<input type="text" placeholder="YY" bind:value={cardYear} maxlength="2" class:error={dateError} on:input={handleInputChange}/>	
				</div>
				{#if dateError}
						<span class="error">{dateError}</span>
				{/if}
				
			</div>
			
			<div>
				<label for="cardCVC">CVC </label>
				<input type="text" placeholder="e.g. 123" bind:value={cardCVC} maxlength="3" class:error = {cvcError} on:input={handleInputChange}/>
				<div>
					{#if cvcError}
						<span class="error">{cvcError}</span>
					{/if}
				</div>
				
			</div>
			
				
		</div>
		
		
		<button on:click={handleSubmit}>Confirm</button>
	</div>
    {:else}
	<div class="thank-you">
		<img src="/icon-complete.svg" alt="Thank You Icon" />
		<h2>THANK YOU!</h2>
		<p>We've added your card details</p>
		<button on:click={() => formSubmitted = false}>Continue</button> 
	</div>
	{/if}
</section>

<style>
	section {
		display: flex;
		align-items: center;	
		min-height: 100vh;
		position: relative;
        font-family: 'Space Grotesk', sans-serif;
		
		
	}
	.bg {
		width: 80%;
		height: 100vh;
		object-fit: cover;
		
	}
	.left-side {
		position: relative;
		width: 40%;
	}


	.card {
		
		border-radius: 10px;
		padding: 20px;
		margin: 20px;
		width: 30%;
		margin-left: auto;
		margin-right: auto;
		
		
	}
	input {
		width: 100%;
		padding: 10px;
		margin:  0.6rem 0;
		border: 1px solid #ccc;
		border-radius: 5px;
		
	}
	button {
		background-color: hsl(278, 68%, 11%);
		color: white;
		padding: 10px 20px;
		border: none;
		border-radius: 5px;
		cursor: pointer;
		width: 100%;
		margin-top: 1rem;
	}
	button:hover {
		background-color: white;
		color: hsl(278, 68%, 11%);
		border: 1px solid hsl(278, 68%, 11%);
        
	}
	.date-cvc {
		display: flex;
		gap: 10px;
		
	}
    .date-space{
        display: flex;
        justify-content: space-between;
    }
	.cards {
		position: absolute;
		top: 50%;
		left: 50%;
	
	}

	.card-front {
		
		position: relative;
		z-index: 2;
		transform: translate(-40%, -120%);
		margin-bottom: 10px;
        
	}

	.card-back {
		
		position: relative;
		transform: translate(-20%, -100%);
		z-index: 1;
        
		
	}
	.date-fields {
		display: flex;
		gap: 10px;
	}

	label {
		font-size: 12px;
		letter-spacing: 2px;
	}

	.card-front-wrapper {
		position: relative;
	}

 
	.card-details {
		position: absolute;
		top: 0;
		left: 0;
		transform: translate(-40%, -100%);
		width: 80%;
		height: 100%;
		padding: 20px;
		display: flex;
		flex-direction: column;
		gap: 0.5rem;
		color: white;
		z-index: 3;
	}

	.card-details-back{
		position: absolute;
		transform: translate(47%, -141%);
		width: 100%;
		height: 100%;
		padding: 20px;
		z-index: 2;
    
	}
	.card-number-back {
		font-size: 1.3rem;
		letter-spacing: 5px;
	}

	.card-logo {
		width: 60px;
		margin-bottom: 5rem;
        margin-top: 1rem;
		
	}

	.card-number {
		font-size: 1.33rem;
		letter-spacing: 0.4rem;
		
	}

	.card-name,
	.card-expiry {
		font-size: 14px;
		text-transform: uppercase;
		margin-top: 10px;
		font-weight: normal;
        letter-spacing: 3px;

        
	}
    
	.error {
		color: red;
		font-size: 12px;
	}

	input.error {
		border-color: red;
	}
   
    .bg-mobile {
        display: none;
    }

    .thank-you{
        margin-left: 15rem;
        text-align: center;
    }

   
    @media (max-width: 375px) {
       
        .bg{
            display: none;
        }
        .bg-mobile {
            display: block;
            width: 100%;
           
        }
        section {
            flex-direction: column;
            align-items: center;
           
        }
        .left-side {
            width: 100%;

        }
       
        .card {
            margin-top: 5rem;
            width: 80%;
        }
        .card-details {
            width: 80%;
            transform: translate(-5%, 40%);

        }
        .card-front {
            transform: translate(-5%, 60%);
          
        }
        .card-back {
            transform: translate(5%, -120%);
            
        }
        .card-front-wrapper {
            transform: translate(-50%, -50%);
        }
        .card-front-wrapper img {
            width: 270px;
        }
        
        .card-details-back {
            transform: translate(70%, -159%);
        }
        .card-number {
            font-size: 1rem;
            letter-spacing: 0.1rem;
        }
        .card-name,
        .card-expiry {
            font-size: 10px;
        }
        .card-number-back {
            font-size: 0.8rem;
        }
        .card-details img {
            width: 50px;
            margin-bottom: 1rem;
            
	    }
     
     
        .card-details-back {
            padding: 10px;
        }

       
        .card-front-wrapper:first-child {
                z-index: 2;
        }

        .card-front-wrapper:last-child {
                z-index: 1;
        }   

        .thank-you {
            text-align: center;
            margin-top: 5rem;
            width: 80%;
            padding: 20px;
            border-radius: 10px;
            background-color: white;
            margin-left: 0rem;
            text-align: none;
            
            
        }
        .thank-you img {
            margin-bottom: 1rem;
        }

    }

    
	
</style>
