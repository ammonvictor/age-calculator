<template>
	<div class="age-calculator">
		<div class="col-md-6">
			<input type="date" class="birthday" v-model="birthday"> {{ birthday }}
			<button class="btn btn-info" @click='processAge()'>Process Age</button>
		</div>
		<div class="col-md-6">
			<div class="display-age">{{ displayAge }}</div>
		</div>
	</div>
</template>

<script>
	export default {
		data(){
			return {
				birthday: "",
				months: [ 'January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December' ],
				profile: {
					age: 0
				},
				today: new Date()
			}
		},
		methods: {
			processAge(){
				// Parse text into a mallable form for JS
			    let bday = new Date(this.birthday);
			    const today = this.today;
				
				let age = today.getFullYear() - bday.getFullYear();
				let monthDiff = today.getMonth() - bday.getMonth();

			  	// If month difference is zero, it is currently their birth month
				if (monthDiff === 0) {

				  this.profile.age = age - 1
				  this.profile.months = today.getMonth() + 1
				  this.profile.days = today.getDate() - bday.getDate()
				  
				} else if (monthDiff < 0) {
				  // If month difference is less the zero, their birth month is -n months away. Thus we can minus one from the age and show how many months to their birth month i.e. x years 8 month old

				  this.profile.age = age - 1
				  this.profile.months = today.getMonth() + 1

				  let daysInCurrentMonth = this.daysInMonth(this.today.getMonth(), this.today.getFullYear())
				  this.profile.days = (daysInCurrentMonth - this.today.getDate()) + bday.getDate()

				} else if (monthDiff > 0) {
				  // if month is greater than zero, their birth month has +n months ago

				  this.profile.age = age
				  this.profile.months = today.getMonth() + 1

				}

			},
			daysInMonth(month, year) {
			  
			  console.log(this.months[month])
			  let x = new Date(year, month+1, 0);
			  console.log(this.months[x.getMonth()])

			  return x.getDate();
			}
		},
		mounted(){
			let bday = new Date(this.birthday)
		  	
		  	// months are zero indexed thus month + 1	
			// console.log("Bday month ", this.months[bday.getMonth()])
			// let daysInBirthdayMonth = this.daysInMonth(bday.getMonth()+1, this.today.getFullYear())
			// 
			// console.log("Current month ", this.months[this.today.getMonth()])

			// days to bday is current month remaining days + n months days 
		},
		computed: {
			displayAge(){
				if (this.profile.age) {
					let str = this.profile.age + ' years ' + this.profile.months + ' months '
					// Math.abs(x)
					if (this.profile.days === 0) {
						// Current date is their birthday
					} else if (this.profile.days < 0) {
						// Bithday is yet to happen
						// this.profile.days = dateDiff
						// str += this.profile.days + ' Days old'
					} else if (this.profile.days > 0) {
						// Birthday has already happened
						// this.profile.days = dateDiff
					}
					return str
				}
			},
		}
	}
</script>