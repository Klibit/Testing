# Testing FE

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
