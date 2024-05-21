# Testing
###############################
BE
php artisan make:modell Book--all
#migration:
use App\Models\Book;
    public function up(): void
    {
        Schema::create('rentals', function (Blueprint $table) {
            $table->id();
            $table->foreignIdFor(Book::class)->constrained();
            $table->date("start");
            $table->date("end");
            $table->timestamps();
        });
    }
#seed
db seeder: $this->call(RentalSeeder::class);
rentalseeder: Rental::factory(15)->create();
rentalfact: class RentalFactory extends Factory
{
    /**
     * Define the model's default state.
     *
     * @return array<string, mixed>
     */
    public function definition(): array
    {
        $bookIds = Book::all()->pluck("id");
        $startDate = fake()->dateTimeBetween('-1 year')->format("Y-m-d");
        $endDate = date("Y-m-d", strtotime($startDate. '+1 week'))
        return [
            "book_id" => fake()->randomElement($bookIds),
            "start_date" => "",
            "end_date" => ""
        ];
    }
}
#controller: php artisan make:controller API/BookController --api
    public function index()
    {
        $books = Book::all(); //get
        return response()->json(["data" => $books]);
    }
    public function store(Request $request, string $id)
    {
        $book = new Book($request->all()); //post
        $book->save;
        return response()->json($book, 201);
    }
    public function rent(string $id)
    {
        $book = Book::find($id);
        if (is_null($book)){
            return response()->json([message => "Nincs ilyen köny"], 404);
        }
        return response()->json("rent");

        $rental = new Rental();
        $rental-> book_id = $id;
        rental->save();

        return response()->json($rental,201);
    }


#routes:
Route::apiResource("/books", BookController::class);
Route::post("/books/{id}/rent", [BookController::class, "rent"])

#request:
    public function rules(): array
    {
        return [
            "title" => "required", 
            "author" => "required", 
            "publishyear" => "required|integer", 
            "pagecount" => "required", 
        ];
    }
    
#Bookmodel:     protected $fillable = ["title","author","publishyear","pagecount"];
    


###############################
FE
import "bootstrap/dist/css/bootsrtrap.min.css";
#Card:
import PropTypes from "prop-types";

function BookCard(props) {
    const url = "http://localhost:8000/api/books";
    const {book} = props

    const rent = async() => {
        const response = await fetch(url + "/" + books.id + "/rent", {
            method: "POST",
            headers: {
                "Accept": "application/json"
            }
        });
        if(response.ok){
            alert("Sikeres foglalás");
        } else {
            const data = await response.json();
            alert(data.message);
        }

    }

    return ( <div className="col">
        <div classname="card" >
            <div className="card-body"></div>
            <h2>{book.title}</h2>
            <p>Kiadási év:</p>
            <img src={"szerzoik/"+book.author+".jpg" } alt={book.author} />
            <button className="btn" onClick={() => rent()}>Kölcsönzés</button>
        </div>
    </div>

     );
}

BookCard.propTypes = {
    book: PropTypes.object.isRequired
}

export default BookCard;

#App
import { useEffect, useState } from 'react'
import reactLogo from './assets/react.svg'
import viteLogo from '/vite.svg'
import './App.css'
import BookCard from './BookCard';

function App() {
  const [books, setBooks] = useState({});
  const url = "http://localhost:8000/api/books";

  const readAllBooks = async () => {
    const response = await fetch(url);
    const data = await response.json();
    setBooks(data.data)

  }
  useEffect(() => {
    readAllBooks();
  }, []);

  return (
    <>
  <header>
    <h1>Címsor</h1>
    <nav></nav>
  </header>
  <main className="container">
    <div className="row row-cols-1 row-cols-md-2 row-cols-lg-3">
      {books.map(book => <BookCard key={book.id} book={book}/>)}
    </div>
    <div className="mt-3" id="newbook">
    <BookForm onSucces={readAllBooks} />
    </div>

  </main>
  <footer>
    <p>Készítette:</p>
  </footer>
    </>
  )
}
export default App

#Bookform:
import PropTypes from "prop-types";

function BookForm(props) {
    const {onSucces} = props;
    const titleRef = useRef(null);
    const url = "http://localhost:8000/api/books";

    const handleSubmit = event =>{
        event.preventDefaul();
        createBook();
        onSuccess();
    }

   const createBook = async() => {
        const book = {
            title : titleRef.current.value
        }
        const response = await fetch(url, {
            method: "POST",
            body: JSON.stringify(book),
            headers: {
                "Content-Type": "application/json",
                "Accept": "application/json",
            }
        })
        if (response.ok){
            onSuccess()
        } else {
            const data = response.json();
        }
        
    }
    return ( 
        <form onSubmit={handleSubmit}>
            <h2>Új könyv</h2>
            <div className="mb-3">
            <label htmlFor="title" className="form-label">Cím</label>
            <input type="text" id="title" className="form-control" ref={titleRef}/>
            </div>

            <button type="submit">Új Könyv</button>
        </form>
     );
}

BookForm.PropTypes = {
    onSucces: PropTypes.func.isRequired
}

export default BookForm;


###############################
program.cs-ben: Mstatic void Main(string[] args) { if args.Contains("--stat")){Statistic.Run}else{applications....}}
Csc
#Book.cs:
int id;
string title;
string authro;

#BookService
project-managenuget-mysql.data

private static string DB_HOST = "localhost;
private static string DB_USER = "root";
private static string DB_PASSWORD = "";
private static string DB_DBNAME = "vizsga";
private MySqlConnection connection;

public BookService(){
MysqlConnectionStringBuilder builder  = new MysqlConnectionStringBuilder();
builder.Server = DB_HOST;
builder.UserID = DB_USER;
builder.Password = DB_PASSWORD

this.connection = new MySQLConnection(builder.ConnectionString);
this.connection.Open();
}

public List<Book> GetBooks(){
List<Book> list = new List<Book>();
string sql = "SELECT * FROM books"
MySQlCommand cmd = this.connection.CreateCOmmand();
cmd.commandText = sql;
using(MySqlDataReader reader  = cmd.ExecuteReader()){
    while(reader.Reaad(){
    int id = reader.GetInt32("id")
    string title = reader.GetString("title")
    string author = reader.GetString("author")
    Book book = new Book(id, title, author)
    list.Add(book);
}
}
return list
}


#Statistic
internal static class Statistic
    static List<Book> books;
public static void Run(){
 Console.WriteLide("Statisztika")
}



###############################
Csg
