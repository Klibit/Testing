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
controller: php artisan make:controller API/BookController --api
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


routes:
Route::apiResource("/books", BookController::class);
Route::post("/books/{id}/rent", [BookController::class, "rent"])

request:
    public function rules(): array
    {
        return [
            "title" => "required", 
            "author" => "required", 
            "publishyear" => "required|integer", 
            "pagecount" => "required", 
        ];
    }
    
Bookmodel:     protected $fillable = ["title","author","publishyear","pagecount"];
    


###############################
FE

###############################
Csc


###############################
Csg



import PropTypes from "prop-types";

function BookCard(props) {
    const {book} = props;
    return (
        <div className="col card">
            <div card-body>
                <h2>{book.title}</h2>
            </div>
        </div>
    );
}

BookVard.proptypes = {
    book: PropTypes.object.isRequired
}

export default BookCard;

_________________

import { useEffect, useState } from 'react'
import './App.css'
import "bootsrtrap/dist/css/bootstrap.css"
import BookCard from '../../BookCard';

function App() {
const [books, setBooks] = useState([]);
const url = "localhost:8000/api/books"

const readAllBooks = () => {
  const response = await fetch(url);
  const data = await response.json();
  setBooks(data.data);

}

useEffect(() => {
  readAllBooks();
}, []);


  return (
    <>
        <header>
    <nav></nav>
    <h1>Petrik könyvtár</h1>
    </header>
    <main className='container'>
      <div className='row'>
        {books.map(book => <BookCard key={book.id} book={book}/>)}

      </div>

    </main>
    </>

    #BE

    


  )
}

export default App
