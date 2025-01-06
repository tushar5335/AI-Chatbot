import json 
burger_sizes = ['single', 'double', 'triple'] 
burger_franchises = ['mac donalds', 'burgerking', 'flaming burger'] 
best_burger_types = ['maharaja mac', 'mac alootikki'] 
burger_palace_types = ['cheeseburst','paneerroyale', 'boss whooper', 'whooper 
jr'] 
flaming_burger_types = ['cheese', 'plain'] 
def validate_order(slots): 
 # Validate BurgerSize 
 if not slots['BurgerSize']: 
 print ('Validating BurgerSize Slot') 
 return { 
 'isValid': False, 
 'invalidSlot': 'BurgerSize' 
 } 
 if slots['BurgerSize']['value']['originalValue'].lower() not in 
burger_sizes: 
 print('Invalid BurgerSize') 
 return { 
 'isValid': False, 
 'invalidSlot': 'BurgerSize', 
 'message': 'Please select a {} burger size.'.format(", 
".join(burger_sizes)) 
 } 
 # Validate BurgerFranchise 
 if not slots['BurgerFranchise']: 
 print('Validating BurgerFranchise Slot') 
 return { 
 'isValid': False, 
 'invalidSlot': 'BurgerFranchise' 
 } 
 if slots['BurgerFranchise']['value']['originalValue'].lower() not in 
burger_franchises: 
 print('Invalid BurgerSize') 
 return { 
 'isValid': False, 
'invalidSlot': 'BurgerFranchise', 
            'message': 'Please select from {} burger franchises.'.format(", 
".join(burger_franchises)) 
        } 
 
    # Validate BurgerType 
    if not slots['BurgerType']: 
        print('Validating BurgerType Slot') 
 
        return { 
            'isValid': False, 
            'invalidSlot': 'BurgerType' 
        } 
 
    # Validate BurgerType for BurgerFranchise 
    if slots['BurgerFranchise']['value']['originalValue'].lower() == 'mac 
donalds': 
        if slots['BurgerType']['value']['originalValue'].lower() not in 
best_burger_types: 
            print('Invalid BurgerType for mac donalds') 
 
            return { 
                'isValid': False, 
                'invalidSlot': 'BurgerType', 
                'message': 'Please select a Best Burger type of {}.'.format(", 
".join(best_burger_types)) 
            } 
 
    if slots['BurgerFranchise']['value']['originalValue'].lower() == 
'burgerking': 
        if slots['BurgerType']['value']['originalValue'].lower() not in 
burger_palace_types: 
            print('Invalid BurgerType for Burger Palace') 
 
            return { 
                'isValid': False, 
                'invalidSlot': 'BurgerType', 
                'message': 'Please select a Burgerking type of {}.'.format(", 
".join(burger_palace_types)) 
            } 
 
    if slots['BurgerFranchise']['value']['originalValue'].lower() == 'flaming 
burger': 
        if slots['BurgerType']['value']['originalValue'].lower() not in 
flaming_burger_types: 
            print('Invalid BurgerType for Flaming Burger') 
 
            return { 
                'isValid': False, 
 'invalidSlot': 'BurgerType', 
 'message': 'Please select a Flaming Burger type of 
{}.'.format(", ".join(flaming_burger_types)) 
 } 
 # Valid Order 
 return {'isValid': True} 
def lambda_handler(event, context): 
 print(event) 
 bot = event['bot']['name'] 
 slots = event['sessionState']['intent']['slots'] 
 intent = event['sessionState']['intent']['name'] 
 order_validation_result = validate_order(slots) 
 if event['invocationSource'] == 'DialogCodeHook': 
 if not order_validation_result['isValid']: 
 if 'message' in order_validation_result: 
 response = { 
 "sessionState": { 
 "dialogAction": { 
 "slotToElicit": 
order_validation_result['invalidSlot'], 
 "type": "ElicitSlot" 
 }, 
 "intent": { 
 "name": intent, 
 "slots": slots 
 } 
 }, 
 "messages": [ 
 { 
 "contentType": "PlainText", 
 "content": order_validation_result['message'] 
 } 
 ] 
 } 
 else: 
 response = { 
 "sessionState": { 
 "dialogAction": { 
 "slotToElicit": 
order_validation_result['invalidSlot'], 
 "type": "ElicitSlot" 
 }, 
 "intent": {
 "name": intent, 
                            "slots": slots 
                        } 
                    } 
                } 
        else: 
            response = { 
                "sessionState": { 
                    "dialogAction": { 
                        "type": "Delegate" 
                    }, 
                    "intent": { 
                        'name': intent, 
                        'slots': slots 
                    } 
                } 
            } 
 
    if event['invocationSource'] == 'FulfillmentCodeHook': 
        response = { 
            "sessionState": { 
                "dialogAction": { 
                    "type": "Close" 
                }, 
                "intent": { 
                    "name": intent, 
                    "slots": slots, 
                    "state": "Fulfilled" 
                } 
 
            }, 
            "messages": [ 
                { 
                    "contentType": "PlainText", 
                    "content": "I've placed your order." 
                } 
            ] 
        } 
 
    print(response) 
    return response <!DOCTYPE html> 
<html lang="en"> 
 
<head> 
    <meta charset="utf-8" /> 
    <meta name="viewport" content="width=device-width, initial-scale=1" /> 
    <title>Burger Buddy</title> 
    <link rel="icon" type="image/x-icon" href="img/favicon.ico" /> 
    <link       
href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/css/bootstrap.min.css" 
rel="stylesheet" integrity="sha384
EVSTQN3/azprG1Anm3QDgpJLIm9Nao0Yz1ztcQTwFspd3yD65VohhpuuCOmLASjC" 
crossorigin="anonymous"> 
    <script type="text/javascript"> 
        (function(d, m){ 
            var kommunicateSettings =  
                {"appId":"20479c7f83ccf3dcbd38c93dd171fea31","popupWidget":tru
 e,"automaticChatOpenOnNavigation":true}; 
            var s = document.createElement("script"); s.type = 
"text/javascript"; s.async = true; 
            s.src = "https://widget.kommunicate.io/v2/kommunicate.app"; 
            var h = document.getElementsByTagName("head")[0]; 
h.appendChild(s); 
            window.kommunicate = m; m._globals = kommunicateSettings; 
        })(document, window.kommunicate || {}); 
    /* NOTE : Use web server to view HTML files as real-time update will not 
work if you directly open the HTML file in the browser. */ 
    </script> 
</head> 
 
<body> 
    <div> 
        <nav class="navbar navbar-expand-sm bg-dark navbar-dark"> 
            <div class="container-fluid"> 
              <a class="navbar-brand" href="#"> 
                <img src="img/robot.png" alt="Avatar Logo" style="width:45px;" 
class="rounded-pill">  
                Burgur Buddy 
              </a> 
              <a href="#menu" class="text-white" style="text-decoration: 
none;"> Our Partners</a> 
               
            </div> 
        </nav> 
 
        <div class="container-fluid text-center p-4"> 
            <h1>Welcome to Burger Buddy</h1> 
 <h3>Now order your favourite Burger from favourite place 
here!</h3> 
        </div> 
    <hr> 
 
    <h2 id = "menu" class="text-center">Our Partner Franchises</h2> 
    <hr> 
    <h3 class="text-center">Burger King</h3> 
    <div class="container overflow-hidden"> 
        <div class="row gy-5"> 
          <div class="col-6"> 
            <div class="p-3 d-flex justify-content-center "> 
                <div class="card" style="width:400px"> 
                    <img class="card-img-top" src="https://external
content.duckduckgo.com/iu/?u=https%3A%2F%2Fres.cloudinary.com%2Fswiggy%2Fimage
 %2Fupload%2Ffl_lossy%2Cf_auto%2Cq_auto%2Cw_208%2Ch_208%2Cc_fit%2Fgq6qbyomdgkuq
 l8aevd1&f=1&nofb=1&ipt=df5c13db64dc6ff4a50c0e68256a28a605918b408278fd95d48dba4
 46bf15e64&ipo=images" alt="Card image"> 
                    <div class="card-body"> 
                      <h4 class="card-title">Paneer Royale</h4> 
                    </div> 
                  </div> 
            </div> 
          </div> 
          <div class="col-6"> 
            <div class="p-3 d-flex justify-content-center "> 
                <div class="card" style="width:400px"> 
                <img class="card-img-top" 
src="https://res.cloudinary.com/swiggy/image/upload/fl_lossy,f_auto,q_auto,w_2
 08,h_208,c_fit/zv7fx1ed433greh6tpvn" alt="Card image"> 
                <div class="card-body"> 
                  <h4 class="card-title">Cheese Burst</h4> 
                </div> 
              </div></div> 
          </div> 
          <div class="col-6"> 
            <div class="p-3 d-flex justify-content-center "> 
                <div class="card" style="width:400px"> 
                <img class="card-img-top" 
src="https://res.cloudinary.com/swiggy/image/upload/fl_lossy,f_auto,q_auto,w_2
 08,h_208,c_fit/sgnlylsa7gqijo2qydq3" alt="Card image"> 
                <div class="card-body"> 
                  <h4 class="card-title">Whooper Jr. 
                  </h4> 
                </div> 
              </div></div> 
          </div> 
          <div class="col-6"> 
            <div class="p-3 d-flex justify-content-center "> 
 <div class="card" style="width:400px"> 
                <img class="card-img-top" 
src="https://res.cloudinary.com/swiggy/image/upload/fl_lossy,f_auto,q_auto,w_2
 08,h_208,c_fit/vriqohxtrafiqxmx6k2p" alt="Card image"> 
                <div class="card-body"> 
                  <h4 class="card-title">Boss Whooper</h4> 
                </div> 
              </div></div> 
          </div> 
        </div> 
      </div> 
      <hr> 
      <h3 class="text-center">Mc Donald's</h3> 
      <div class="container overflow-hidden"> 
          <div class="row gy-5"> 
            <div class="col-6"> 
              <div class="p-3 d-flex justify-content-center "> 
                  <div class="card" style="width:400px"> 
                      <img class="card-img-top" 
src="https://res.cloudinary.com/swiggy/image/upload/fl_lossy,f_auto,q_auto,w_2
 08,h_208,c_fit/46dc2ed3b890dfaf9ab7590f5ff67c50" alt="Card image"> 
                      <div class="card-body"> 
                        <h4 class="card-title">Mc Aloo Tikki</h4> 
                      </div> 
                    </div> 
              </div> 
            </div> 
            <div class="col-6"> 
              <div class="p-3 d-flex justify-content-center "> 
                  <div class="card" style="width:400px"> 
                  <img class="card-img-top" 
src="https://res.cloudinary.com/swiggy/image/upload/fl_lossy,f_auto,q_auto,w_2
 08,h_208,c_fit/4d91eb6e4a6053046f924af5f6784985" alt="Card image"> 
                  <div class="card-body"> 
                    <h4 class="card-title">Maharaja mac</h4> 
                  </div> 
                </div></div> 
            </div> 
          </div> 
        </div> 
        <hr> 
    <h3 class="text-center">Burger King</h3> 
    <div class="container overflow-hidden"> 
        <div class="row gy-5"> 
          <div class="col-6"> 
            <div class="p-3 d-flex justify-content-center "> 
                <div class="card" style="width:400px"> 
 <img class="card-img-top" 
src="https://res.cloudinary.com/swiggy/image/upload/fl_lossy,f_auto,q_auto,w_2
 08,h_208,c_fit/vp9vngykvtsoxk5rhhde" alt="Card image"> 
                    <div class="card-body"> 
                      <h4 class="card-title">Plain</h4> 
                    </div> 
                  </div> 
            </div> 
          </div> 
          <div class="col-6"> 
            <div class="p-3 d-flex justify-content-center "> 
                <div class="card" style="width:400px"> 
                <img class="card-img-top" 
src="https://res.cloudinary.com/swiggy/image/upload/fl_lossy,f_auto,q_auto,w_2
 08,h_208,c_fit/243ff944e7aa1025698bc362700d66b8" alt="Card image"> 
                <div class="card-body"> 
                  <h4 class="card-title">Cheesey</h4> 
                </div> 
              </div></div> 
          </div> 
        </div> 
      </div> 
      <hr> 
== 
 
    <h3 class="text-center">Burger Sizes (# patties)</h3><br> 
    <ul class="d-flex justify-content-evenly"> 
        <list class="fs-4 border p-4 bg-secondary bg-gradient text
white">Single</list> 
        <list class="fs-4 border p-4 bg-secondary bg-gradient text
white">Double</list> 
        <list class="fs-4 border p-4 bg-secondary bg-gradient text
white">Triple</list> 
    </ul> 
</body> 
 
</html> <script type="text/javascript"> 
(function(d, m){ 
var kommunicateSettings =  
{"appId":"20479c7f83ccf3dcbd38c93dd171fea31","popupWidget":tru
 e,"automaticChatOpenOnNavigation":true}; 
var s = document.createElement("script"); s.type = 
"text/javascript"; s.async = true; 
s.src = "https://widget.kommunicate.io/v2/kommunicate.app"; 
var h = document.getElementsByTagName("head")[0]; 
h.appendChild(s); 
window.kommunicate = m; m._globals = kommunicateSettings; 
})(document, window.kommunicate || {}); 
/* NOTE : Use web server to view HTML files as real-time update will not 
work if you directly open the HTML file in the browser. */ 
</script>
