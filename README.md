Xamarin-Azure
===========


<br />

##XAMARIN FORMS
1.	Start Visual Studio > File > New Project > Visual C# > Cross-Platform >Xamarin.Forms(Portable) > Give a name to the Project (MSRIT) >Ok.
2.	Right Click MSRIT(Portable) > Add > New Item > Forms Xaml Page > PageOne.xaml > OK.
3.	MSRIT(Portable) > App.cs > Comment/Remove all the lines of code inside the constructor - App() and add the below line.
 
 MainPage = new NavigationPage(new PageOne());
 
4.	Open PageOne.xaml > Copy paste the below code inside the ContentPage Node.
 

<Grid>


     <Button x:Name="myButton" Text="Hello World" Font="20" Clicked="myButton_Click" HorizontalOptions="Center" VerticalOptions      ="Center" />
  
  
  </Grid>
 
 
5.	Open PageOne.xaml.cs and insert the below code inside the namespace.
 
    void myButton_Click(object sender, EventArgs args)
    {
        count++;
        myButton.Text = count.ToString();
    }
 
##CALCULATOR
 
1.	Right Click on MSRIT(Solution) > Add New Project > Visual C# > Class Library (Xamarin.Forms)>Name it Calculator
2.	Remove all code. Keep the public class Calculator
3.	Add functions for Addition,Substraction,Multiplication and Division.
Eg:
 public int Add(int x, int y)
 
        {
            int z = x + y;
 
            return z;
        }
 
4.	Build the Calculator.PCL project.
5.	Add the Calculator.PCL.dll found under (Calculator.PCL > bin > Debug folder) as a Reference to MSRIT(Portable).
 
    6. Add the Below Code inside the Constructor.
 
Label result = new Label { Text = "Result" };
            public PageOne()
            {
                InitializeComponent();
 
                var num1 = new Entry { Placeholder = "Number 1" };
 
                var num2 = new Entry { Placeholder = "Number 2" };
 
 
 
                var add = new Button
 
                {
 
                    Text = "Add",
 
                    TextColor = Color.White,
 
                    BackgroundColor = Color.FromHex("77D065")
 
                };
 
                var sub = new Button
 
                {
 
                    Text = "Sub",
 
                    TextColor = Color.White,
 
                    BackgroundColor = Color.FromHex("77D065")
 
                };
 
                var mul = new Button
 
                {
 
                    Text = "Mul",
 
                    TextColor = Color.White,
 
                    BackgroundColor = Color.FromHex("77D065")
 
                };
 
                var div = new Button
 
                {
 
                    Text = "Div",
 
                    TextColor = Color.White,
 
                    BackgroundColor = Color.FromHex("77D065")
 
                };
 
                add.Clicked += (sender, args) =>
 
                {
 
                    var calc = new Calculator.PCL.Calculator();
 
 
 
                    var n1 = int.Parse(num1.Text);
 
                    var n2 = int.Parse(num2.Text);
 
                    result.Text = calc.Add(n1, n2).ToString();
 
                };
 
                sub.Clicked += (sender, args) =>
 
                {
 
                    var calc = new Calculator.PCL.Calculator();
 
                    var n1 = int.Parse(num1.Text);
 
                    var n2 = int.Parse(num2.Text);
 
                    result.Text = calc.Subtract(n1, n2).ToString();
 
                };
 
                mul.Clicked += (sender, args) =>
 
                {
 
                    var calc = new Calculator.PCL.Calculator();
 
                    var n1 = int.Parse(num1.Text);
 
                    var n2 = int.Parse(num2.Text);
 
                    result.Text = calc.Multiply(n1, n2).ToString();
 
                };
 
                div.Clicked += (sender, args) =>
 
                {
 
                    var calc = new Calculator.PCL.Calculator();
 
                    var n1 = int.Parse(num1.Text);
 
                    var n2 = int.Parse(num2.Text);
 
 
 
 
 
                    result.Text = calc.Divide(n1, n2).ToString();
 
                };
 
                var sl1 = new StackLayout
 
                {
 
                    Orientation = StackOrientation.Horizontal,
 
                    Children = { add, sub }
 
                };
 
                var sl2 = new StackLayout
 
                {
 
                    Orientation = StackOrientation.Horizontal,
 
                    Children = { mul, div }
 
                };
 
 
                this.Padding = new Thickness(10, Device.OnPlatform(20, 0, 0), 10, 5);
 
                this.Content = new StackLayout
                {
                    Children =
                    {
                        num1, num2, sl1, sl2, result
                    }
                };
 
            }
 
 
 
 
 


##OBSERVABLE COLLECTION
 
1.	PageOne.xaml and Insert the below Code.
 
 
    <AbsoluteLayout HorizontalOptions="FillAndExpand" VerticalOptions="FillAndExpand">
 
      <StackLayout
 
        AbsoluteLayout.LayoutFlags="All"
 
        AbsoluteLayout.LayoutBounds="0,0,1,1">
 
        <ListView ItemsSource="{Binding Sessions}"
 
                  VerticalOptions="FillAndExpand"
 
                  HasUnevenRows="False"                 
 
                  CachingStrategy="RecycleElement"
 
                  x:Name="myList">
 
          <ListView.ItemTemplate>
 
            <DataTemplate>
             
              <ImageCell ImageSource="{Binding Images}" Text="{Binding Desc}"/>
 
            </DataTemplate>
 
          </ListView.ItemTemplate>
 
        </ListView>
 
      </StackLayout>
    
 
    </AbsoluteLayout>
 
  </ContentPage.Content>
 
</ContentPage>
 
 
 
2.	PageOne.xaml.cs and insert the below code.
 
 public class Session
 
    {
        public string Id { get; set; }
        public string Name { get; set; }
        public string Desc { get; set; }
        public string Images { get; set; }
        
    }
 
 public ObservableCollection<Session> Sessions { get; set; }
        public ICommand GetSessionsCommand { get; set; }
 
        
        async Task GetSessions()
 
        {
 
            
                using (var client = new System.Net.Http.HttpClient())
 
                {
 
                    var json = await client.GetStringAsync("https://apchin-mobileapp.azurewebsites.net/api/Check");
                    
                    //Deserialize json
 
                    var items = JsonConvert.DeserializeObject<List<Session>>(json);
 
                    myList.ItemsSource = items;
 
                    //Load sessions into list
 
 
                }
 
        }
 
 
===============
###Under the constructor call
 
 Sessions = new ObservableCollection<Session>();
 
 Title = "Sessions";
 
 GetSessions();


##AUTHENTICATION
1.	Create an interface
 
 public interface IAuthenticate
 
    {
 
        Task<bool> Authenticate(int Provider);
 
    }
 
 
 public static IAuthenticate Authenticator { get; private set; }
  public static void Init(IAuthenticate authenticator)
 
        {
 
            Authenticator = authenticator;
 
        }
 
 
###SHARED/COMMON
        MobileServiceClient client = new MobileServiceClient("https://apchin-mobileapp.azurewebsites.net");
 
 
 
        public MobileServiceAuthenticationProvider ServiceProvider { get; private set; }
 
 
###ANDROID
 
 private MobileServiceUser user;
 
 
user = await client.LoginAsync(this,
 
                    ServiceProvider);
 
                if (user != null)
 
                {
 
                    message = string.Format("you are now signed-in as {0}.",
 
                        user.UserId);
 
                    success = true;
 
                }
 
 
 
###WINDOWS
 
 public async Task<bool> Authenticate()
 
{
     if (user == null)
 
                {
 
                    user = await client.LoginAsync(ServiceProvider);
 
                    if (user != null)
 
                    {
 
                        success = true;
 
                        message = string.Format("You are now signed-in as {0}.", user.UserId);
 
                    }
 
    }
            }
 
 
iOS
 
{
         if (user == null)
 
                {
 
                    user = await client.LoginAsync(UIApplication.SharedApplication.KeyWindow.RootViewController,
 
                       ServiceProvider);
 
                    if (user != null)
 
                    {
 
                        message = string.Format("You are now signed-in as {0}.", user.UserId);
 
                        success = true;
 
                    }
 
                }
            }
 
 


 

