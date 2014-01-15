---
layout: post
title:  "Switchable One-Time or Recurring Paypal Donation Button"
categories: WordPress
---

If you need a Paypal donation button, and you want the user to be able to select either one-time or recurring donations, its not so simple. These require completely different form submissions to Paypal.  

So I wrote this form, and an accompanying jQuery script. It lets the user switch back and forth between one time and recurring donation options.

If this is installed on a WordPress or other CMS site, it is better set up as a plugin. A site owner does not need to lose all functionality just because he/she switches themes.

{% highlight html %}
<div class="donationContainer">
    <form action="https://www.paypal.com/cgi-bin/webscr" method="post" target="_blank" id="recurringHiddenInputs"> 
        <input type="hidden" name="item_name" value="Donation"> 
        <input type="hidden" name="business" value="donations@non-profit.org"> 
        <input type="hidden" name="lc" value="US"> 
        <input type="hidden" name="no_shipping" value="1"> 
        <input type="hidden" name="currency_code" value="USD"> 
        <input type="hidden" name="return" value="http://your-site.org/thankyou/"> 
        <input type="hidden" name="p3" value="1" class="recurring"> 
        <input type="hidden" name="src" value="1" class="recurring"> 
        <input type="hidden" name="no_note" value="0" class="recurring"> 
        <input type="hidden" name="bn" value="PP-DonationsBF:btn_donateCC_LG.gif:NonHostedGuest" class="recurring"> 
        <table>
            <tbody>
                <tr>
                    <td colspan="2" style="padding: 0 1px;"><h4>Donate by Paypal</h4></td>
                </tr>
                <tr>
                    <td>Please select <span>*</span></td>
                    <td>
                        <div class="radio-choice">
                            <input type="radio" name="cmd" value="_xclick-subscriptions" id="severalTimes" checked>
                            <label for="severalTimes">Recurring Donation</label>
							<input type="radio" name="cmd" value="_donations" id="oneTime">
							<label for="oneTime">One-time Donation</label>
						</div>
                    </td>
                </tr>
                <tr>
                    <td class="firstcol"><label for="amountInput">Donation amount ($) <span>*</span></label></td>
                    <td id="donationAmount"><input type="text" name="a3" placeholder="25" class="recurring" id="amountInput"></td>
                </tr>
                <tr id="recurringFrequency">
                    <td class="recurring"><label for="frequency">Frequency</label></td>
                    <td class="recurring"><select name="t3" id="frequency">
                        <option value="M" selected="selected">Monthly</option>
                        <option value="Y">Yearly</option>
                    </select></td>
                </tr>
                <tr>
                    <td><input type="hidden" name="on0" value="Program"> <label for="os0">Gift made to:</label></td>
                    <td><select name="os0" id="os0">
                        <option value="General Operating Budget">General Operating Fund</option>
                        <option value="Scholarship Fund">Scholarship Fund</option>
                        <option value="Equipment Fund">Equipment Fund</option>
                        <option value="Bus Maintenance">Bus Maintenance Fund</option>
                    </select></td>
                </tr>
                <tr>
                    <td><input type="hidden" name="on1" value="Dedicated to"> <label for="os1">Donation made<br>in honor of:</label></td>
                    <td><input name="os1" type="text" placeholder="name" id="os1"></td>
                </tr>
                <tr>
                    <td colspan="2" class="paypal-button">
                    <input type="image" src="https://www.paypalobjects.com/en_US/i/btn/btn_donate_LG.gif" name="submit" alt="Donate by PayPal"></td>
                </tr>
                <tr>
                    <td colspan="2" class="required"><span>* required</span></td>
                </tr>
            </tbody>
        </table>
    </form>
</div>
{% endhighlight %}
It requires this bit of jQuery to switch the forms:
{% highlight js %}
jQuery(document).ready(function($) {
	$("input[id='oneTime']").change(function() {
		if ($(this).is(':checked')) { 
			$('.recurring').remove();
			$('#donationAmount').append('<input type="text" name="amount" placeholder="25" class="oneTime" id="amountInput">');
		} 
	});
	$("input[id='severalTimes']").change(function() {
		if ($(this).is(':checked')) {
			$('.oneTime').remove();
			$('#recurringFrequency').append('<td class="recurring"><label for="frequency">Frequency</label></td>
				<td class="recurring"><select name="t3" id="frequency"> 
				<option value="M" selected="selected">Monthly</option> 
				<option value="Y">Yearly</option></select></td>');
			$('#recurringHiddenInputs').append(' <input type="hidden" name="p3" value="1" class="recurring"> 
				<input type="hidden" name="src" value="1" class="recurring"> 
				<input type="hidden" name="no_note" value="0" class="recurring"> 
				<input type="hidden" name="bn" value="PP-DonationsBF:btn_donateCC_LG.gif:NonHostedGuest" class="recurring"> ');
			$('#donationAmount').append('<input type="text" name="a3" placeholder="25" class="recurring">');
		}
	})
});
{% endhighlight %}

