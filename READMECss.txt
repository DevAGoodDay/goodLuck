.stack-items {
    grid-template-columns: 1fr 1fr;
    display: grid;
    max-width: 800px;
    }

@media (max-width: 768px){
    .stack-items {
        grid-template-columns: 1fr 1fr;
    }
}
@media (min-width: 769px) {
    .stack-items{
        grid-template-columns: 1fr 1fr 1fr;
    }
}

.stack {
    margin-top: 20px;
    display: flex;
    gap: 8px;
}

.stack-items h2 {
    text-shadow: 5px 5px var(--primary);
}

#contact textarea {
    border: 1px solid #ccc;
    border-radius: 5px;
}