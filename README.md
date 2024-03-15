    public double AlphaBetaWithTT(Board board, int depth, double alpha, double beta, bool player_is_bot)
    {
        if (depth <= 0)
            return Evaluate_Position(board);

        if (Transpositions.ContainsKey(board.ZobristKey))
            return Transpositions[board.ZobristKey];

        if (board.IsDraw()) //Prefer to lose than make a boring draw, also helps with no repetion draws in better position
            return -2;

        Move[] moves = board.GetLegalMoves();
        double maxEval = player_is_bot ? double.MinValue : double.MaxValue;

        foreach (Move move in moves)
        {
            board.MakeMove(move);
            double evaluation = AlphaBetaWithTT(board, depth - 1, alpha, beta, !player_is_bot);
            board.UndoMove(move);

            if (player_is_bot)
            {
                maxEval = Math.Max(maxEval, evaluation);
                alpha = Math.Max(alpha, evaluation);
            }
            else
            {
                maxEval = Math.Min(maxEval, evaluation);
                beta = Math.Min(beta, evaluation);
            }

            if (beta <= alpha)
                break;
        }

        return maxEval;
    }
