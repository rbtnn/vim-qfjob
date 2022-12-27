
# vim-qfjob

__Sample__
```
command! -nargs=*  GitGrep  :call s:gitgrep(<q-args>)

function! s:gitgrep(q_args) abort
  let cmd = ['git', '--no-pager', 'grep', '--no-color', '-n', '--column'] + split(a:q_args, '\s\+')
  let id = qfjob#start(cmd, {
    \ 'title': 'git grep',
    \ 'line_parser': function('s:gitgrep_line_parser'),
    \ })
  " If you want to stop the job, call qfjob#stop().
  " call qfjob#stop(id)
endfunction

function s:gitgrep_line_parser(line) abort
  let m = matchlist(a:line, '^\(.\{-\}\):\(\d\+\):\(\d\+\):\(.*\)$')
  if !empty(m)
    return {
      \ 'filename': m[1],
      \ 'lnum': m[2],
      \ 'col': m[3],
      \ 'text': m[4]),
      \ }
  else
    return { 'text': a:line, }
  endif
endfunction
```
