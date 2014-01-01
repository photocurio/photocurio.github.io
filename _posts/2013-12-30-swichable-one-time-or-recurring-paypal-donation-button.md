---
layout: post
title:  "Switchable One-Time or Recurring Paypal Donation Button"
categories: WordPress
---

You'll find this post in your `_posts` directory - edit this post and re-build (or run with the `-w` switch) to see your changes!
To add new posts, simply add a file in the `_posts` directory that follows the convention: YYYY-MM-DD-name-of-post.ext.

Jekyll also offers powerful support for code snippets:

{% highlight html %}
<div class="donationContainer">
        <form action="https://www.paypal.com/cgi-bin/webscr" method="post" target="_blank" id="recurringHiddenInputs">
            <input type="hidden" name="item_name" value="Donation"> <input type="hidden" name="business" value="don@trekkers.org"> <input type="hidden" name="lc" value="US"> <input type="hidden" name="no_shipping" value="1"> <input type="hidden" name="currency_code" value="USD"> <input type="hidden" name="return" value="http://trekkers.org/test-donate/"> <input type="hidden" name="p3" value="1" class="recurring"> <input type="hidden" name="src" value="1" class="recurring"> <input type="hidden" name="no_note" value="0" class="recurring"> <input type="hidden" name="bn" value="PP-DonationsBF:btn_donateCC_LG.gif:NonHostedGuest" class="recurring">

            <table>
                <tbody>
                    <tr>
                        <td colspan="2" style="padding: 0 1px;">
                            <h4>Donate by Paypal</h4>
                        </td>
                    </tr>

                    <tr>
                        <td>Please select <span>*</span></td>

                        <td>
                            <div class="radio-choice">
                                <input type="radio" name="cmd" value="_xclick-subscriptions" id="severalTimes" checked><label for="severalTimes">Recurring Donation</label>
                            </div><input type="radio" name="cmd" value="_donations" id="oneTime"><label for="oneTime">One-time Donation</label>
                        </td>
                    </tr>

                    <tr>
                        <td class="firstcol"><label for="amountInput">Donation amount ($) <span>*</span></label></td>

                        <td id="donationAmount"><input type="text" name="a3" placeholder="25" class="recurring" id="amountInput"></td>
                    </tr>

                    <tr id="recurringFrequency">
                        <td class="recurring"><label for="frequency">Frequency</label></td>

                        <td class="recurring"><select name="t3" id="frequency">
                            <option value="M" selected="selected">
                                Monthly
                            </option>

                            <option value="Y">
                                Yearly
                            </option>
                        </select></td>
                    </tr>

                    <tr>
                        <td><input type="hidden" name="on0" value="Program"> <label for="os0">Gift made to:</label></td>

                        <td><select name="os0" id="os0">
                            <option value="General Operating Budget">
                                General Operating Fund
                            </option>

                            <option value="Scholarship Fund">
                                Scholarship Fund
                            </option>

                            <option value="Equipment Fund">
                                Equipment Fund
                            </option>

                            <option value="Bus Maintenance">
                                Bus Maintenance Fund
                            </option>
                        </select></td>
                    </tr>

                    <tr>
                        <td><input type="hidden" name="on1" value="Dedicated to"> <label for="os1">Donation made<br>
                        in honor of:</label></td>

                        <td><input name="os1" type="text" placeholder="name" id="os1"></td>
                    </tr>

                    <tr>
                        <td colspan="2" class="paypal-button"><input type="image" src="https://www.paypalobjects.com/en_US/i/btn/btn_donate_LG.gif" name="submit" alt="PayPal - The safer, easier way to pay online!"></td>
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
{% highlight javascript %}
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
			$('#recurringFrequency').append('<td class="recurring"><label for="frequency">Frequency</label></td><td class="recurring"><select name="t3" id="frequency"><option value="M" selected="selected">Monthly</option><option value="Y">Yearly</option></select></td>');
			$('#recurringHiddenInputs').append(' <input type="hidden" name="p3" value="1" class="recurring"> <input type="hidden" name="src" value="1" class="recurring"> <input type="hidden" name="no_note" value="0" class="recurring"> <input type="hidden" name="bn" value="PP-DonationsBF:btn_donateCC_LG.gif:NonHostedGuest" class="recurring"> ');
			$('#donationAmount').append('<input type="text" name="a3" placeholder="25" class="recurring">');
		}
	})
});
{% endhighlight %}


Check out the [Jekyll docs][jekyll] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll's GitHub repo][jekyll-gh].

[jekyll-gh]: https://github.com/mojombo/jekyll
[jekyll]:    http://jekyllrb.com
